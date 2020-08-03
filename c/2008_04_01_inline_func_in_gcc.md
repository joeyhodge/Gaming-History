# inline func in GCC

今天碰到个问题，如何在 C 中使用 inline func。inline func是 C99 的标准，不过 gcc 很早就提供支持了。

对于 inline func 的林林总总，下面的文字有详细的描述：

 * [http://www.greenend.org.uk/rjk/2003/03/inline.html][1]

对于比较偷懒的家伙，下面给出 gcc 中使用 inline func 的方法：

```C
static inline void foo() { ... }      // in header file
```

而对于 C99 应该这样用：

```C
// foo.h
inline void foo() { ... }

// foo.c
#include "foo.h"
extern void foo();
```

然后其他文件里面就可以随便用了。

不过，即使 gcc -std=c99 也不支持 C99 的标准方法，嘿嘿~~~~

[1]:http://www.greenend.org.uk/rjk/2003/03/inline.html
