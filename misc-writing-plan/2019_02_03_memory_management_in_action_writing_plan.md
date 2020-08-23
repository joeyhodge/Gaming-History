# 《内存管理实践之路》写作计划

## 缘起

一直很喜欢侯捷(jjhou)，大学时期学完了他的《[Word排版艺术][2]》，就老想着自己写本书，自己排版。

毕业几年之后，学了点皮毛（2008年左右），不知天高地厚，打算开坑写《[Memory Management In Action - Vol.1][1]》，写了个开头，就写不动了。

原计划出两卷

 * 卷1，写手工内存分配
 * 卷2，写GC

卷1 的 README 里，还这样写着

```
MMIA1 - Memory Management In Action Vol.1

Algorithm & Implementation of manual-memory allocator (no gc stuff)
```


## 缘聚

心血来潮，和 [辉哥][3] 吹水，突然发现GC相关的两本书，都是 [Richard Jones][4] 写的。想起自己的太监之作《[Memory Management In Action - Vol.1][1]》，要不，还是把坑填了吧。也刷一波出书成就。

卷1、卷2 考虑合并，内容改为分析所有实际项目中的算法。我可是实战派。:-)

大纲如下

```
嵌入式操作系统：手工分配内存（ucOS-II）
通用操作系统：虚拟内存管理（Windows的实现方案、OpenBSD的实现方案、Linux的实现方案）
libc：malloc / free（jemalloc、tcmalloc）
C++：STL（stl allocator，jjhou 的《STL源码分析》，小块内存分配器）
CLR：Generation 算法（.net core & mono 两套实现）
Python：refcount GC（相当简陋）
Lua：Mark-and-Swap
```


## 缘灭

TODO，等我写完书，来填这一段。


[1]:https://github.com/kasicass/kasicass/tree/master/books/article/Memory_Management_In_Action_Vol1
[2]:https://book.douban.com/subject/1193565/
[3]:https://github.com/SixGodZhang
[4]:https://www.cs.kent.ac.uk/people/staff/rej/gc.html
