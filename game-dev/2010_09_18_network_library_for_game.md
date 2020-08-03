[game-dev] 关于网络库的选择

对于网络游戏，一般都用些啥现成的库呢？

## 客户端业务模型

```C
while (1)
{
    TimerMgr->DoTask();
    NetworkMgr->DoTask();
    RenderMgr->DoTask();
    ....
}
```

其中 NetworkMgr->DoTask() 就是尝试收一下数据包，如果有就处理下。

## 服务端业务模型

与http请求不同，游戏数据包有很强的时序性，所以适合 event-dispatching 的方式。

```
while (1)
{
    select(...);
    ... network, timer, etc. event arrive, dealing it
}
```

对于一个 connection，需要知道 connection make, data arrive, connection lost 三种 event。

## 网络库，如何选择？

个人意见

 * 客户端，没有语音聊天、P2P需求的，tcp这种东西，自己封装下就好。
 * 服务端，封装下 socket，配合 libevent 使用。在此基础上弄个 protobuf 之类的来做 rpc。

其它网络库

 * 为啥不用 ACE，太重，太复杂。没必要。下面这篇评论，我是比较赞同的。但 ACE 绝对是兵器谱上最值得学习的一把武器。对 ACE 的评价，看 [这里][1]。
 * 为啥不用 Ice，也太重，太复杂。其实游戏不需要那么复杂的网络功能。不过我对于 Ice 的研究还很粗浅，不深入评论。
 * RakNet，据说是有公司在用的，我没用过。
 * ZeroMQ，适合用于搭建多进程的服务器，让你爽快地进程间通讯，但 client ==> server 的连接管理，还需要自己做。

## Updated - 2018.11.17

客户端/服务器，都可以用 [libuv][2]。

[1]:http://blog.csdn.net/Solstice/archive/2010/03/10/5364096.aspx
[2]:https://libuv.org/
