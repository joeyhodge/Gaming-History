# [FreeBSD] 8.0 原来已去掉了 KSE

FreeBSD 8.0

kse 是 FreeBSD 支持 M:N 线程模型的库，不过因为太复杂，已经被去掉了。

 * [参考1][1]
 * [参考2][2]

貌似 FreeBSD 还是坚持得比较久的，Solaris, NetBSD 早就放弃了 M:N 模型。目前主流的实现都变成 1:1 模型了。

 * [http://en.wikipedia.org/wiki/Thread_(computer_science)][3]

```
libkse, src/lib/libkse, 废弃
libc_r, src/lib/libc_r, 废弃
libthr, src/lib/libthr
```

```
$ ls -l /usr/lib/libpthread*
lrwxr-xr-x  1 root  wheel   8 11 21 22:54 /usr/lib/libpthread.a -> libthr.a
lrwxr-xr-x  1 root  wheel   9 11 21 22:54 /usr/lib/libpthread.so -> libthr.so
lrwxr-xr-x  1 root  wheel  10 11 21 22:54 /usr/lib/libpthread_p.a -> libthr_p.a
```

可以看出 libpthread 默认都链接到 libthr。但有个奇怪的问题，就是 gcc -pthread 和 gcc -lthr 得到的 gprof 结果，居然不一样。

而且在 thread func 里面做的事情，居然也 gprof 不到。不知道是不是我自己系统的问题。

一些想法得到验证 :-)

 * [http://www.freebsdchina.org/forum/viewtopic.php?t=50846][4]


[1]:http://www.sunchangming.com/blog/?p=2660
[2]:http://docs.freebsd.org/cgi/getmsg.cgi?fetch=501996+0+/usr/local/www/db/text/2008/freebsd-current/20080316.freebsd-current
[3]:http://en.wikipedia.org/wiki/Thread_(computer_science)
[4]:http://www.freebsdchina.org/forum/viewtopic.php?t=50846


