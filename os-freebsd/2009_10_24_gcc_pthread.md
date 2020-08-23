# [FreeBSD] -lpthread 与 -pthread

FreeBSD 6.2 上碰到过 -lpthread 编译的程序，用 top 看不到 cpu 占用率的情况，或看到的占用率不准确。

换成 -lthr 即可。

而今天看到这里介绍，使用 -pthread (注意，没有'l')，gcc 会自动链接系统当前版本推荐的 thread lib 以及对应的 thread safe 的 c func。其实 -pthread 就是链接到 /usr/lib/libpthread.so，至于此 .so 是哪个线程库的 link，就是系统决定了。

[http://www.zeroc.com/forums/help-center/4334-ice-freebsd.html][1]

gcc manual 里面也有关于此的讨论

[http://gcc.gnu.org/onlinedocs/libstdc++/manual/using_concurrency.html][2]

注意，FreeBSD 中，其实包含了 libc_r, libthr, libpthread(libkse) 三个版本的多线程库。

 * lib_r, Reentrant C Library，最老的版本，基本废弃。
 * libthr, 1:1 thread model，8.0 开始，它就是默认的库。
 * libpthread(libkse), M:N thread model，6.x, 7.x 下的默认库。

其中 libpthread 默认使用的是 PTHREAD_SCOPE_PROCESS，而 libthr 用的是 PTHREAD_SCOPE_SYSTEM。理论上来说，后者在多核状态下，且服务器主要就跑你的程序的时候，能更好的利用 cpu。

 * [线程的属性(3) -- scope & inherit sched (REALTIME)][3]

恩，top为何不通过的原因，有空还要细细研究下。

[1]:http://www.zeroc.com/forums/help-center/4334-ice-freebsd.html
[2]:http://gcc.gnu.org/onlinedocs/libstdc++/manual/using_concurrency.html
[3]:https://github.com/kasicass/blog/blob/master/pthread/2009_12_28_scope_and_inherit_sched.md
