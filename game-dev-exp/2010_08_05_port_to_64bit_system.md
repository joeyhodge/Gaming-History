[game-dev] 关于移植到64-bit系统

[http://en.wikipedia.org/wiki/64-bit][1]

把 C 程序移植到 64-bit 系统下，并不复杂。32 / 64-bit 的区别，就是（针对*nix系统）

* 32-bit 系统：sizeof(int) == 4, sizeof(long) == 4
* 64-bit 系统：sizeof(int) == 4, sizeof(long) == 8

64-bit 系统上，void * 和 int 不能直接转换了。其他，好像就没啥需要注意的地方了。

换到 64-bit 系统的好处，恩，有了很多很多内存，哈哈。

[1]:http://en.wikipedia.org/wiki/64-bit
