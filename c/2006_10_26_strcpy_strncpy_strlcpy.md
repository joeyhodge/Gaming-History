# [ANSI C] strcpy & strncpy & strlcpy

## 基础知识

### strcpy

我们知道，strcpy 是依据 \0 作为结束判断的，如果 to 的空间不够，则会引起 buffer overflow。strcpy 常规的实现代码如下（来自 OpenBSD 3.9）：

```C
char *
strcpy(char *to, const char *from)
{
    char *save = to;

    for (; (*to = *from) != '\0'; ++from, ++to);
    return(save);
}
```

但通常，我们的 from 都来源于用户的输入，很可能是非常大的一个字符串，因此 strcpy 不够安全。

### strncpy

在 ANSI C 中，strcpy 的安全版本是 strncpy。

```C
char *strncpy(char *s1, const char *s2, size_t n);
```

但 strncpy 其行为是很诡异的（不符合我们的通常习惯）。标准规定 n 并不是 sizeof(s1)，而是要复制的 char 的个数。一个最常见的问题，就是 strncpy 并不帮你保证 \0 结束。

```C
char buf[8];
strncpy( buf, "abcdefgh", 8 );
```

看这个程序，buf 将会被 "abcdefgh" 填满，但却没有 \0 结束符了。

另外，如果 s2 的内容比较少，而 n 又比较大的话，strncpy 将会把之间的空间都用 \0 填充。这又出现了一个效率上的问题，如下：

```C
char buf[80];
strncpy( buf, "abcdefgh", 79 );
```

上面的 strncpy 会填写 79 个 char，而不仅仅是 "abcdefgh" 本身。

strncpy 的标准用法为：（手工写上 \0）

```C
strncpy(path, src, sizeof(path) - 1);
path[sizeof(path) - 1] = '\0';
len = strlen(path);
```
### strlcpy

```C
// Copy src to string dst of size siz.  At most siz-1 characters
// will be copied.  Always NUL terminates (unless siz == 0).
// Returns strlen(src); if retval >= siz, truncation occurred.
size_t
strlcpy(char *dst, const char *src, size_t siz);
```

而使用 strlcpy，就不需要我们去手动负责 \0 了，仅需要把 sizeof(dst) 告之 strlcpy 即可：

```C
size_t len;
len = strlcpy(path, src, sizeof(path));

if ( len >= sizeof(path) )
    printf("src is truncated.");
```

并且 strlcpy 传回的是 strlen(str)，因此我们也很方便的可以判断数据是否被截断。


## 一点点历史

strlcpy 并不属于 ANSI C，至今也还不是标准。

strlcpy 来源于 OpenBSD 2.4，之后很多 unix-like 系统的 libc 中都加入了 strlcpy 函数，我个人在 FreeBSD、Linux 里面都找到了 strlcpy。（Linux使用的是 glibc，glibc里面有 strlcpy，则所有的 Linux 版本也都应该有 strlcpy）

但 Windows 下是没有 strlcpy 的。

另外，strlcat 与 strlcpy 类似，这里就不讨论了。


## 参考文献

 * [strlcpy and strlcat - consistent, safe, string copy and concatenation.][1]

[1]:https://www.sudo.ws/todd/papers/strlcpy.html
