# [game-dev] 书籍推荐

本来是写给新同学作为参考的，难得写这么多文字，贴到 blog 上自我陶醉下。咔咔。

## 学习资料/推荐书目

一个人不可能把所有知识都掌握无余，本文只希望达到“在学习某一领域软件知识时，可很快找到一些比较不错的参考书籍”的目的。开卷有益 :-)

对于程序员来说，多读、多写代码可以收获很多。现在有很多优秀的开源项目，可以选取与工作相关的一两个项目，认真研读其代码。有时比读书，提高更快。

Source code always tell the truth.


## 体系篇

```
+------------+------------------------+----------------+
|   GUI(L4)  |      Network/DB(L5)    |   2D/3D (L6)   |
+------------+------------------------+----------------+
|       系统API(L2)         |     compiler/tool(L3)    |
+------------------------------------------------------+
|                    硬件/操作系统(L1)                  |
+------------------------------------------------------+
```

### L1 - Layer 1 (硬件/操作系统)

此 Layer 主要是介绍操作系统的实现。

《Intel 64 and IA-32 Architectures Software Developer's Manuals》

 * [https://software.intel.com/en-us/articles/intel-sdm][1]
 * x64/x86 体系结构权威手册，可以下载到 pdf 版本。

《深入理解计算机系统》

 * [http://www.douban.com/subject/1230413/][2]
 * 计算机软硬件体系结构深入浅出的介绍。

《LINUX内核源代码情景分析》

 * [http://www.douban.com/subject/1240321/][3]

《Windows内核情景分析》

 * [http://www.douban.com/subject/3715700/][4]
 * 两本情景分析，是以代码为实例，解说了两大操作系统的具体实现。实践性比较强。

《深入解析Windows操作系统》

 * [http://www.douban.com/subject/2031396/][5]
 * Windows官方著作，理论多，实践少。

《自己动手写操作系统》/《Orange S:一个操作系统的实现》

 * [http://www.douban.com/subject/1422377/][6]
 * [http://www.douban.com/subject/3735649/][7]
 * 一个作者同一系列的两本书，看一本即可。操作系统的入门材料。


### L2 - Layer 2 (系统API)

系统级对象、API的使用，比如 Process, Thread, Mutex, Socket 等等。

《Windows核心编程》

 * [http://www.douban.com/subject/3235659/][8]

《Windows多线程程序设计》

 * [http://book.douban.com/subject/1231702/][9]

《UNIX环境高级编程》

 * [http://www.douban.com/subject/1692629/][10]

《UNIX Programming FAQ》作为上一本书的补充

 * [http://www.svbug.com/documentation/comp.unix.programmer-FAQ/faq_toc.html][11]

《The Linux Programming Interface》

 * [http://book.douban.com/subject/4292217/][12]


## L3 - Layer 3 (libc/compiler)

使用高级语言，用好编译器、调试工具是必不可少的基础。至于是否需要研究编译器原理，则只是个人爱好了。

《Compilers: Principles, Techniques, and Tools》

 * [http://www.douban.com/subject/1866231/][13]

《Advanced Compiler Design and Implementation》

 * [http://www.douban.com/subject/1821532/][14]

《Modern Compiler Implementation in C》

 * [http://www.douban.com/subject/1886911/][15]

三本讲解编译器实现的大部头，喜爱编译器原理的同学可以参考下。

《Linkers and Loaders》

 * [http://www.douban.com/subject/1436811/][16]
 * 链接和装载方面的权威理论著作。
 * 中文版下载，[http://www.oldlinux.org/oldlinux/viewthread.php?tid=10713][17]

《程序员的自我修养 -- 链接、装载与库》

 * [http://www.douban.com/subject/3652388/][18]
 * 可以看作是《Linkers and Loaders》的升级中文版。

《软件调试》/《Windows高级调试》

 * [http://www.douban.com/subject/3088353/][19]
 * [http://www.douban.com/subject/3781532/][20]
 * Windows 下的两本 debugging 宝典。

《Makefile/GCC/GDB 学习》

 * 网上很多资料，可以任意 google
 * 当然，gcc/gdb manual 是最详细的，虽然有点枯燥。

linux 下几个性能、内存检查的常用工具

 * [http://sourceware.org/binutils/docs/gprof/index.html][21]
 * [http://valgrind.org/][22]
 * [http://dmalloc.com/][23]


## L4 - Layer 4 (GUI) 

GUI app 算是 desktop app，虽然是做游戏，但也免不了写点小工具，比如：地图编辑器、资源打包工具等等。

所以 GUI 知识也是需要的。这里介绍的都是 C/C++ 的 GUI 库，一般我们的做法是把 C/C++ 库封装到脚本(lua/python)，直接通过脚本写具体的逻辑。MFC 是 windows 官方的古老东西，廉颇老矣，可以不用学习了。

当然，其实用 C# 做界面也是很方便的。Java 也行，就是有点慢。

《Programming Windows Fifth Edition》

 * [http://www.douban.com/subject/1456779/][24]
 * 理解 C/C++ 版的 win app 是如何运作的，第五版是最经典的一个版本。

《深入浅出MFC》

 * [http://www.douban.com/subject/1482240/][25]
 * 这本书其实并不会告诉你MFC怎么用，而让你了解到一个 C++ GUI framework 应该具备哪些最基本的元素。如：消息传递、RTTI等等。

《MFC Windows程序设计》

 * [http://www.douban.com/subject/1128016/][26]
 * MFC 每个控件的详细介绍，不过不熟悉 MFC 的同学可以不用学了。

wxWidgets / Qt / GTK+

 * [http://www.wxwidgets.org/][27]
 * [https://www.qt.io/][28]
 * [http://www.gtk.org/][29]
 * 三者是跨平台的UI库，wx与MFC比较像。学一即可满足日常需求，实际工作中，我们用 wx 比较多。

2013.09 updated：

 * MFC 已经逝去，WPF 才是王道。离开邪恶的 C++，投入 C# 的怀抱吧。


## L5 - Layer 5 (Network/DB)

Network，如果只从 socket api 来说，只属于“系统API”，但网络游戏中，服务端程序还是非常重要的，所以我把其单独分为一个 Layer，且同时涵盖了网络、数据存储两者。

《TCP/IP 详解》 Vol 1/2/3

 * [http://www.douban.com/subject/1099252/][30]
 * [http://www.douban.com/subject/1231729/][31]
 * [http://www.douban.com/subject/1095214/][32]
 * IPv4 原理的权威书籍
 * 看卷1、卷2就好，卷3用不上

《UNIX Network Programming》 Vol 1/2

 * [http://www.douban.com/subject/1174626/][33]
 * [http://www.douban.com/subject/1231788/][34]
 * UNIX 网络编程的权威著作

libevent / RakNet / ACE / Ice

 * [http://www.monkey.org/~provos/libevent/][35]
 * [https://github.com/facebookarchive/RakNet][36]
 * [http://www.cse.wustl.edu/~schmidt/ACE.html][37]
 * [http://www.zeroc.com/][38]
 * 四个跨平台的网络封装库，其中 libevent 是最轻量级的，而 RakNet 是专为游戏设计。
 * ACE/Ice 都是比较重量级的，可以阅读其代码，学习一些网络框架的设计思想。

MaNGOS

 * [http://getmangos.com/][39]
 * WOW 的模拟服务端，C++ 代码还是很清晰的。

Mud OS / LDMud

 * [http://www.mudos.org/][40]
 * [http://www.bearnip.com/lars/proj/ldmud.html][41]
 * 古老的 mud 游戏的服务端，虽然代码老了点，结构乱了点，但也是很多网络游戏的服务端雏形。

《深入浅出MySQL》

 * [http://www.douban.com/subject/3012338/][42]
 * 公司同事的作品，MySQL非常好的入门书籍。:-)

《High Performance MySQL》

 * [http://www.douban.com/subject/3101726/][43]

SQLite / MySQL

 * [http://www.sqlite.org/][44]
 * [http://www.mysql.com/][45]
 * SQLite 是基于文件的DB，配合 GUI 程序用来做存储，还是很不错的。


## L6 - Layer 6 (2D/3D)

《游戏编程大师技巧》 Vol 1/2

 * [http://www.douban.com/subject/1230286/][46]
 * [http://www.douban.com/subject/1321769/][47]
 * 两本书本别介绍了 2D/3D 的基础。非常非常好的入门资料，特别是 3D 那本，介绍了写3D程序所需要的数学/3D知识。

《3D Engine Design》

 * [https://book.douban.com/subject/3554163/][48]
 * 介绍了主流的3D游戏引擎应如何设计。作者同时实现了 WildMagic，一款开源的3D引擎。

WindSoul

 * [http://blog.codingnow.com][49]
 * [http://www.codingnow.com/2000/index.html][50]
 * 云风GG的力作，2D游戏引擎。

HGE

 * [http://hge.relishgames.com/][51]
 * 2D 引擎的另一个发展方向，用 3D 渲染 2D。（利用硬件加速）

Box2D

 * [http://www.box2d.org/][52]
 * 2D 物理引擎

IrrLicht

 * [http://irrlicht.sourceforge.net/][53]
 * 代码清晰，结构简单，适合入门阅读。

ogre

 * [http://www.ogre3d.org/][54]
 * 结构清晰，但重量级的开源3D引擎

Bullet / ODE

 * [http://www.bulletphysics.com/wordpress/][55]
 * [http://www.ode.org/][56]
 * 两款开源的3D物理引擎


## 语言篇

语言是工具，语言没有好坏，只有是否适用，以及你对其有多少的熟练度。

越熟悉，才能写出结构更好、效率更高的代码。

我只列出游戏部常用的开发语言，C#/Java/Lisp 不在此列。:-)


### C/C++                                            

C++ 是门不算古老但足够复杂的语言。实践中，高级的 template 特性的滥用，会导致代码不好维护。

所以在考虑深入 C++ 的高级特性前，可以先读读此 blog。Just thinking, 项目中需要这么多高级特性么？

 * [http://blog.csdn.net/pongba/archive/2007/05/16/1611593.aspx][57]

下面我就不列出我认为有点“偏”的 C++ 图书了。虽然只想列几本重点的，也发现列了不少。- -#

进入 C++ 11，C++ 又进化了许多，虽然依旧 evil。看看 pongba 同学对 C++ 11 的实践：

 * [http://mindhacks.cn/2012/08/27/modern-cpp-practices/][58]

Modern C++，

 * [http://msdn.microsoft.com/en-us/library/vstudio/hh279654.aspx][59]

《The C Programming Language》《C++ Primer》《The C++ Programming Language》
 * [http://www.douban.com/subject/1236999/][60]
 * [http://www.douban.com/subject/2696025/][61]
 * [http://www.douban.com/subject/1767741/][62]
 * 三本基础书，C++ 的读其中一本即可。

《C陷阱与缺陷》《C专家编程》《C/C++ 深层探索》
 * [http://www.douban.com/subject/1102097/][63]
 * [http://www.douban.com/subject/2377310/][64]
 * [http://www.douban.com/subject/1232030/][65]
 * C/C++ 的提高篇

《Effective C++》《More Effective C++》《Effective STL》
 * [http://www.douban.com/subject/1453373/][66]
 * [http://www.douban.com/subject/1457891/][67]
 * [http://www.douban.com/subject/1792179/][68]
 * Effective 三套件

《C++ 编程规范》
 * [http://www.douban.com/subject/1480481/][69]
 * 编码规范的书很多，看一本就好，其他的东西，实践中慢慢体会。

《C++标准程序库自修教程与参考手册》《STL 源码剖析》
 * [http://www.douban.com/subject/1110941/][70]
 * [http://www.douban.com/subject/1110934/][71]
 * STL 最好的两本参考手册。

《Imperfect C++》《深度探索C++对象模型》
 * [http://www.douban.com/subject/1470838/][72]
 * [http://www.douban.com/subject/1091086/][73]
 * 提升 C++ 内力的两本书。

《道法自然：面向对象实践指南》《C++实践之路》
 * [http://www.douban.com/subject/1231194/][74]
 * [http://www.douban.com/subject/1102104/][75]
 * 上面的书，如果都还偏理论的话，这两本书就是用实践说话了。


### Lua                                                

我最喜欢这种简单的语言 :-) 书籍少，好学，但又很实用。

《Programming Lua, 2nd》
 * [http://www.douban.com/subject/3076942/][76]

《Lua Reference》
 * [http://www.lua.org/manual/5.1/][77]


### Python

Python 的书也很多，看完下面两本，剩下的再参考官方 manual 也就差不多了。

《Learning Python》
 * [http://www.douban.com/subject/3243372/][78]
 * 基础篇

《Python Cookbook》
 * [http://www.douban.com/subject/1418172/
 * 提高篇


### Java

最近开始对 Java 又产生了点兴趣（2011.03），也列一些 Java 的书吧。

《Java Power Tools》
 * [http://book.douban.com/subject/3034363/][80]
 * 编译、构建、bug追踪、自动测试、持续集成。介绍你每天都必须使用的工具，All in One book。


## 算法/数据结构篇

算法涉及的范畴也很广泛，这里偏重介绍数据结构的基础书籍。

《算法导论》
 * [http://www.douban.com/subject/1152912/][81]
 * 理论基础篇

《Art of Computer Programming》
 * [http://www.douban.com/subject/1418402/][82]
 * 没啥可介绍的，算法著作中的《葵花宝典》。


## 软件设计篇

设计模式就是些名词，方便大家交流时，更准确地了解对方用了怎样的程序结构。

《设计模式》《Head First Design Pattern》《大话设计模式》
 * [http://www.douban.com/subject/1099305/][83]
 * [http://www.douban.com/subject/1400656/][84]
 * [http://www.douban.com/subject/2334288/][85]
 * 第一本是经典著作，但有点理论化，稍显晦涩。后两本则是通俗易懂型的，老外的例子和我们生活上有点差异，
 * 好像第三本更适合我们，呵呵。

《重构》
 * [http://www.douban.com/subject/1229923/][86]

《UNIX编程艺术》
 * [http://www.douban.com/subject/1467587/][87]
 * 软件设计的 KISS 原则 (Keep It Simple, Stupid)

《Pattern-Oriented Software Architecture》 Vol 1/2/3/4/5
 * [http://www.douban.com/subject/1232017/][88]
 * [http://www.douban.com/subject/1137259/][89]
 * [http://www.douban.com/subject/1444890/][90]
 * 一共五卷，不过中文版似乎还只有三卷。其中卷二对网络框架的设计有比较大的参考意义。


## 开发方法篇

不同的开发方法论，适用于不同规模的开发团队。传统的软件工程，比较适合需求固定的庞大的系统。而游戏开发与之相反，团队规模小而需求变化快，所以 Agile Development (敏捷方法) 比较适合我们。

各种敏捷方法中，我个人比较喜欢 scrum，公司好几个工作室也在实施。下面是个人的一点总结，仅供参考：

 * [https://github.com/kasicass/blog/blob/master/scrum/2008_02_28_about_xp.md][91]

最后一句话：方法是死的，灵活运用，找到属于自己团队最佳的实践。

《代码大全》
 * [http://www.douban.com/subject/1477390/][92]
 * [http://blog.codingnow.com/cloud/CodeComplete][93]


《人月神话》
 * [http://www.douban.com/subject/2230248/][94]
 * 说起项目管理，这本书总还是要去读读的。

《I. M. Wright's Hard Code》
 * [http://www.douban.com/subject/3259433/][95]
 * 来自 M$ 的项目管理经验书

《敏捷迭代开发管理者指南》
 * [http://www.douban.com/subject/1801394/][96]

《Agile Software Development with Scrum》
 * [http://www.douban.com/subject/1153186/][97]
 * Scrum 发起者的著作，用于理解 scrum 的各种概念

《超越传统的软件开发》
 * [http://www.douban.com/subject/1220623/][98]
 * 上面的都是国外和尚念的经，这里强烈推荐一本国人的作品，写得很实在。可惜网上已经买不到了，公司图书馆里还有得借。


## 其他人推荐的书单

Milo Yip 推荐的游戏程序员书单
 * [https://github.com/miloyip/game-programmer][99]

Books For Game Developers
 * [http://mrelusive.com/books/books.html][100]

THE COMPUTER GRAPHICS LIBRARY
 * [http://fabiensanglard.net/Computer_Graphics_Principles_and_Practices/index.php][101]

附上pongba同学的书单
 * [http://mindhacks.cn/2011/11/04/how-to-interview-a-person-for-two-years/][102]



[1]:https://software.intel.com/en-us/articles/intel-sdm
[2]:http://www.douban.com/subject/1230413/
[3]:http://www.douban.com/subject/1240321/
[4]:http://www.douban.com/subject/3715700/
[5]:http://www.douban.com/subject/2031396/
[6]:http://www.douban.com/subject/1422377/
[7]:http://www.douban.com/subject/3735649/
[8]:http://www.douban.com/subject/3235659/
[9]:http://book.douban.com/subject/1231702/
[10]:http://www.douban.com/subject/1692629/
[11]:http://www.svbug.com/documentation/comp.unix.programmer-FAQ/faq_toc.html
[12]:http://book.douban.com/subject/4292217/
[13]:http://www.douban.com/subject/1866231/
[14]:http://www.douban.com/subject/1821532/
[15]:http://www.douban.com/subject/1886911/
[16]:http://www.douban.com/subject/1436811/
[17]:http://www.oldlinux.org/oldlinux/viewthread.php?tid=10713
[18]:http://www.douban.com/subject/3652388/
[19]:http://www.douban.com/subject/3088353/
[20]:http://www.douban.com/subject/3781532/
[21]:http://sourceware.org/binutils/docs/gprof/index.html
[22]:http://valgrind.org/
[23]:http://dmalloc.com/
[24]:http://www.douban.com/subject/1456779/
[25]:http://www.douban.com/subject/1482240/
[26]:http://www.douban.com/subject/1128016/
[27]:http://www.wxwidgets.org/
[28]:https://www.qt.io/
[29]:http://www.gtk.org/
[30]:http://www.douban.com/subject/1099252/
[31]:http://www.douban.com/subject/1231729/
[32]:http://www.douban.com/subject/1095214/
[33]:http://www.douban.com/subject/1174626/
[34]:http://www.douban.com/subject/1231788/
[35]:http://www.monkey.org/~provos/libevent/
[36]:https://github.com/facebookarchive/RakNet
[37]:http://www.cse.wustl.edu/~schmidt/ACE.htm
[38]:http://www.zeroc.com/
[39]:http://getmangos.com/
[40]:http://www.mudos.org/
[41]:http://www.bearnip.com/lars/proj/ldmud.html
[42]:http://www.douban.com/subject/3012338/
[43]:http://www.douban.com/subject/3101726/
[44]:http://www.sqlite.org/
[45]:http://www.mysql.com/
[46]:http://www.douban.com/subject/1230286/
[47]:http://www.douban.com/subject/1321769/
[48]:https://book.douban.com/subject/3554163/
[49]:http://blog.codingnow.com
[50]:http://www.codingnow.com/2000/index.html
[51]:http://hge.relishgames.com/
[52]:http://www.box2d.org/
[53]:http://irrlicht.sourceforge.net/
[54]:http://www.ogre3d.org/
[55]:http://www.bulletphysics.com/wordpress/
[56]:http://www.ode.org/
[57]:http://blog.csdn.net/pongba/archive/2007/05/16/1611593.aspx
[58]:http://mindhacks.cn/2012/08/27/modern-cpp-practices/
[59]:http://msdn.microsoft.com/en-us/library/vstudio/hh279654.aspx
[60]:http://www.douban.com/subject/1236999/
[61]:http://www.douban.com/subject/2696025/
[62]:http://www.douban.com/subject/1767741/
[63]:http://www.douban.com/subject/1102097/
[64]:http://www.douban.com/subject/2377310/
[65]:http://www.douban.com/subject/1232030/
[66]:http://www.douban.com/subject/1453373/
[67]:http://www.douban.com/subject/1457891/
[68]:http://www.douban.com/subject/1792179/
[69]:http://www.douban.com/subject/1480481/
[70]:http://www.douban.com/subject/1110941/
[71]:http://www.douban.com/subject/1110934/
[72]:http://www.douban.com/subject/1470838/
[73]:http://www.douban.com/subject/1091086/
[74]:http://www.douban.com/subject/1231194/
[75]:http://www.douban.com/subject/1102104/
[76]:http://www.douban.com/subject/3076942/
[77]:http://www.lua.org/manual/5.1/
[78]:http://www.douban.com/subject/3243372/
[79]:http://www.douban.com/subject/1418172/
[80]:http://book.douban.com/subject/3034363/
[81]:http://www.douban.com/subject/1152912/
[82]:http://www.douban.com/subject/1418402/
[83]:http://www.douban.com/subject/1099305/
[84]:http://www.douban.com/subject/1400656/
[85]:http://www.douban.com/subject/2334288/
[86]:http://www.douban.com/subject/1229923/
[87]:http://www.douban.com/subject/1467587/ 
[88]:http://www.douban.com/subject/1232017/
[89]:http://www.douban.com/subject/1137259/
[90]:http://www.douban.com/subject/1444890/
[91]:https://github.com/kasicass/blog/blob/master/scrum/2008_02_28_about_xp.md
[92]:http://www.douban.com/subject/1477390/
[93]:http://blog.codingnow.com/cloud/CodeComplete
[94]:http://www.douban.com/subject/2230248/
[95]:http://www.douban.com/subject/3259433/
[96]:http://www.douban.com/subject/1801394/
[97]:http://www.douban.com/subject/1153186/
[98]:http://www.douban.com/subject/1220623/
[99]:https://github.com/miloyip/game-programmer
[100]:http://mrelusive.com/books/books.html
[101]:http://fabiensanglard.net/Computer_Graphics_Principles_and_Practices/index.php
[102]:http://mindhacks.cn/2011/11/04/how-to-interview-a-person-for-two-years/
