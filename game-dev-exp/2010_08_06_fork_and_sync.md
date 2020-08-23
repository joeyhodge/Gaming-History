[game-dev] 利用 fork 来定期保存数据

## 基本思想

游戏中，我们希望将某一时刻的玩家数据，一次保存起来，保证没有数据复制。

要把数据保存起来，如果前期设计不好，可能需要到很多模块去抠抠挖挖，把数据序列化好，然后写到磁盘。这个序列化的过程可能就需要花费不少的cpu time。

怎么办呢？如果内存够大（上64-bit system），我们可以先 fork，让parent process继续服务玩家，而 child process 则序列化、写磁盘。利用操作系统来给我们做数据的 snapshot。

## 实现细节

用 fork，有几个要注意的地方，如下：

 * parent process 需要 signal(SIGCHLD, SIG_IGN);。否则 child process 退出后，会产生 zombie process。但要注意，SIG_IGN会影响一些函数的行为，比如：用了 SIG_IGN system() 必然返回 -1（因为 waitpid 失败）
 * child process 只会有一个 thread (parent process 调用 fork 的那个 thread)，其他的 thread 都自动没了。
 * child process 不要做任何逻辑，只做存盘的事情就好。

资料参考，看 Steven 老大的 APUE

 * ch08 Process Control  ==>  8.3 fork Function
 * ch10 Signals ==> 10.7 SIGCLD Semantics
 * ch12 Thread Control  ==>  12.9 Threads and fork

ps.

 1. FreeBSD上，不处理 signal(SIGCHLD, SIG_IGN)，还可以 rfork(RFFDG | RFPROC | RFNOWAIT)，通过 RFNOWAIT 把 child process 与 parent 脱钩。**参见 3.**
 2. fork进程大量写数据的同时，如果主进程也做了一些io操作，很可能导致主进程block io。看 [这里][1]
 3. 厄，rfork() 的行为和 fork() 除了 RFNOWAIT 还是有一些不一样的。rfork() 出来的进程，会保留所有的 thread，而不是仅保留 mainthread。

看看 rfork() 行为：

```C
#include <pthread.h>
#include <unistd.h>
#include <stdio.h>

void *mythread(void *_)
{
    sleep(100);
    return (void *)0;
}

int main()
{
    int pid;
    pthread_t tid;
    pthread_create(&tid, NULL, mythread, NULL);

    pid = rfork(RFFDG | RFPROC | RFNOWAIT);
    // pid = fork();
    if ( pid > 0 )
    {
        printf("child = %d\n", pid);
        sleep(100);
    }
    else if ( pid == 0 )
    {
        sleep(100);
    }

    return 0;
}
```

通过 kill -SIGABRT pid 把 child process coredump，通过 gdb 来看。

```
#0  0x28171c2f in nanosleep () from /lib/libc.so.7        # rfork()
[New Thread 28217140 (LWP 100128)]
[New Thread 28201140 (LWP 100125)]
[New LWP 100108]

#0  0x28171c2f in nanosleep () from /lib/libc.so.7        # fork()
[New Thread 28201140 (LWP 100079)]
```

所有 thread 保留。rfork() 比较危险，不熟悉不要用。

[1]:https://github.com/kasicass/blog/blob/master/freebsd/2010_11_22_optimize_program.md
