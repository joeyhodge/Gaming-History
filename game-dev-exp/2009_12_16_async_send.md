# [game-dev] 关于网络发包异步化

## 改进前

 * 只有一个 MainThread, produce/compress/crypt/send packet 都在此线程中。
 * 当玩家增多，数据量增大后，MainThread 明显花费了大量 cpu 在 compress, crypt, send 中。
 * 玩法逻辑处理不过来，导致玩家感觉“卡”。


## 改进后

 * MainThread, produce packet
 * SendThread, compress, crypt, send packet
 * 基本想法就是把 block 住的事情交给线程，反正目前的机器都是多核的，不利用就浪费了。
 * 以后还可以根据测试数据，需要的话，可以多启几个 SendThread。


## 具体实现

 * 我期望 SendThread 的行为越简单越好，不要与 MainThread 有太多的交互。
 * 采用 producer/consumer 模型，MainThread 通过 FIFO-queue 把任务交给 SendThread。
 * SendThread 在 send() 时，如果出错，简单丢弃数据包即可。SendThread 不会给 MainThread 任何反馈。

实现的指令：

 * STC_SEND <fd> <data>, 将data compress/crypt, 并发送给fd
 * STC_CLOSEFD <fd>, 关闭fd
 * STC_QUIT, 让线程主动退出

如果 compress/crypt 需要一些初始化参数，可以增加几条单独的线程指令，来控制初始化行为


## 情况讨论

对一些情况下，线程行为的讨论。

```
Case 1:
  玩家主动退出
  MainThread  ==>  read()等于0 or -1(某些错误)  ==>  STC_CLOSEFD

Case 2:
  服务器主动踢玩家
  MainThread  ==>  删除对此fd的监听  ==>  STC_CLOSEFD

Case 3:
  引擎退出
  MainThread  ==>  STC_QUIT  ==>  等待 SendThread 退出后，在退出 MainThread

Case 4:
  SendThread send() 时得到 EPIPE
  说明此fd已经断开连接，MainThread 自然会受到通知，如 Case 1
  丢弃要发送的数据包即可

Case 5:
  SendThread send() 得到 EAGAIN
  说明客户端自己处理不过来了，或者是出了某些错误(拔网线??)，丢包吧。
  客户端定期给服务器发心跳包，长期不心跳的客户端，踢掉，如 Case 2
```

这里处理得不太好的就是 Case 5，有可能因为丢包，导致某个客户端的逻辑出错，而客户端还在正常发心跳包。

我认为此情况很少出现，不处理了。


## 应用案例

大话、梦幻服务端的网络发包线程，整体设计就是这样的。


## 参考实现

thr_queue 实现，参考 [这里][1]。

```C
#include "thr_queue.h"

#define STC_SEND      1
#define STC_CLOSEFD   2
#define STC_QUIT      3

typedef struct stcmd_base_s
{
  int op;
} stcmd_base_t;

typedef struct stcmd_fd_s
{
  int op;
  int fd;
} stcmd_fd_t;

typedef struct stcmd_data_s
{
  int op;
  int fd;
  char *data;
  size_t len;
} stcmd_data_t;

// ------ Global Var -------

static thr_queue_t *workq;
static pthread_t tid;

// ------ Private Func -------

// send thread log func
static void sendthread_log(char *fmt, ...)
{
  static FILE *fp = NULL;
  time_t tm;
  char tmbuf[80];
  va_list args;

  if (fp == NULL)
  {
    fp = fopen("send_thread.log", "w");
    if (fp == NULL)
    {
      printf("fopen(\"send_thread.log\") fail: %s\n", strerror(errno));
      exit(-1);
    }
  }

  // time
  time(&tm);
  fprintf(fp, "[%.24s] ", ctime_r(&tm, tmbuf));

  // log content
  va_start(args, fmt);
  vfprintf(fp, fmt, args);
  va_end(args);
  fflush(fp);
}

static void send_cmd(int op)
{
  stcmd_base_t *cmd = (stcmd_base_t *)malloc(sizeof(stcmd_base_t));
  cmd->op = op;
  thr_queue_push(workq, cmd);
}

static void send_fd_cmd(int op, int fd)
{
  stcmd_fd_t *cmd = (stcmd_fd_t *)malloc(sizeof(stcmd_fd_t));
  cmd->op = op;
  cmd->fd = fd;
  thr_queue_push(workq, cmd);
}

static void send_data_cmd(int op, int fd, const char *data, size_t len)
{
  stcmd_data_t *cmd = (stcmd_data_t *)malloc(sizeof(stcmd_data_t));
  cmd->op   = op;
  cmd->fd   = fd;

  cmd->data = (char *)malloc(len);
  memcpy(cmd->data, data, len);
  cmd->len  = len;

  thr_queue_push(workq, cmd);
}

static void mysend(int fd, const char *data, size_t len)
{
  ssize_t nbytes;
  size_t sendn;

  sendn = 0;
  while (1)
  {
    nbytes = write(fd, data+sendn, len-sendn);
    if (nbytes < 0)
    {
      char errbuf[80];
      if (strerror_r(errno, errbuf, sizeof(errbuf)) == 0)
        sendthread_log("write fail, fd=%d, estr=%s\n", fd, errbuf);
      return;
    }

    sendn += nbytes;
    if (sendn == len)
      return;
  }
}

static void *send_thread(void * _)
{
  void *out = NULL;

  while (1)
  {
    thr_queue_pop(workq, &out);

    switch (((stcmd_base_t *)out)->op)
    {
    case STC_SEND:
      {
      stcmd_data_t *cmd = (stcmd_data_t *)out;
      mysend(cmd->fd, cmd->data, cmd->len);
      free(cmd->data);
      }
      break;

    case STC_CLOSEFD:
      close(((stcmd_fd_t *)out)->fd);
      break;

    case STC_QUIT:
      free(out);
      return (void *)0;

    default:
      break;
    }

    free(out);
    out = NULL;
  }

  return (void *)0;
}

// ------ Public Func -------

void send_thread_close_fd(int fd)
{
  send_fd_cmd(STC_CLOSEFD, fd);
}

void send_thread_send_raw(int fd, const char *data, size_t len)
{
  send_data_cmd(STC_SEND, fd, data, len);
}

void send_thread_init(int max_online)
{
  workq = thr_queue_create(max_online * max_online / 2);
  pthread_create(&tid, NULL, send_thread, NULL);
}

void send_thread_shutdown()
{
  send_cmd(STC_QUIT);

  pthread_join(tid, NULL);
  thr_queue_destroy(workq);
}
```

[1]:https://github.com/kasicass/blog/blob/master/pthread/2010_01_13_FIFO_queue.md
