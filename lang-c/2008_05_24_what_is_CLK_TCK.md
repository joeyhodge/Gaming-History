# CLK_TCK 是虾米？

我在 CentOS 上编译 lua posix 失败，原因是 CLK_TCK 不存在。

```C
#define pushtime(L,x)           lua_pushnumber(L,((lua_Number)x)/CLK_TCK)
```

CLK_TCK 是啥呢？其实就是 CLOCKS_PER_SEC，不过 CLK_TCK 已然废弃，新的代码中不要使用就好。

参见 [http://msdn.microsoft.com/en-us/library/8001551c(VS.80).aspx][1]

```
CLOCKS_PER_SEC, CLK_TCK 
    The time in seconds is the value returned by the clock function, divided by CLOCKS_PER_SEC. CLK_TCK is equivalent, but considered obsolete.
```

参见 [http://www.chemie.fu-berlin.de/chemnet/use/info/libc/libc_17.html][2]

```
int CLK_TCK
    This is an obsolete name for CLOCKS_PER_SEC.
```

[1]:http://msdn.microsoft.com/en-us/library/8001551c(VS.80).aspx
[2]:http://www.chemie.fu-berlin.de/chemnet/use/info/libc/libc_17.html
