# C#, Set Sail~

一直没有系统地学习过 Java or C#，最近认识了好多 C# 大佬。抱紧大腿，好好学习，refactoring 这部分知识。

C++ 已经[学腻了][4]，总得找点新乐子。:-)

列一些书单，C# 学习之旅，[扬帆启航][5]。


## Father of C#

我学习一门新语言，喜欢先看看语言作者。没有啥成就的作者，设计的语言，基本可以忽略。

[Anders Hejlsberg][28]，Father of C#，为写编译器、设计编程语言而生的男人。一辈子都在写编译器，好像也挺无聊的 =_=!。

 * 早年在 Borland 写 Turbo Pascal、Delphi
 * 去了 Microsoft，为对抗 Java，设计了 C#
 * 最后设计了 TypeScript，给 JavaScript 这货，打个 C# patch

从 DOS 时代迁徙过来的程序员，一定知道 Borland 的 Turbo Pascal 和 Turbo C++。我记得，高中奥赛用的就是 Turbo Pascal。

进入 Windows 时代，Delphi 如雷贯耳。写 Win32 应用程序，Visual C++、MFC，易用性上，就是渣渣。用过 Delphi(Pascal) 的人，才知道写 Win32 程序可以托托拽拽点一下就可以的。然后 Microsoft 出个 Visual Basic，总算扳回一局。

Basic 是以简单见长的语言，到了 VB.NET，那就不叫 Basic 了，那是硬插上 OO 翅膀的怪胎。哎呀，歪楼了。

对当年 Microsoft 和 Borland 的编译器圣战感兴趣的同学，可以去读读《[Borland创奇][30]》。对 Delphi 设计上感兴趣，可以读读《[VCL构架剖析][31]》。如今传奇已不再，而 [Anders Hejlsberg][28] 还在。:-)

1990年代后期，Java 横空出世，Microsoft 鸭梨山大，C++ 明显已显疲态，为了圈住开发者，不流失到 Sun 的土地上，Microsoft 重金挖了 [Anders Hejlsberg][28]，为 .NET Framework 插上翅膀。

如何挖牛人？"double，来不来？"，哈哈。

    Microsoft offered Anders Hejlsberg a signing bonus of US$500,000 and stock options. 
    Microsoft doubled the bonus to US$1,000,000 after Borland made a counter-offer. 
    Hejlsberg left Borland in October 1996.

为数不多的 Microsoft Technical Fellow 之中，[Anders Hejlsberg][28] 的名字赫然在列。想知道 Microsoft 有哪些 TF-Boys，看 [这里][29]。


## 入门篇

刷一下语言基础，刷一下 IL 基础。

 * 《[The C# Programming Language][27]》
 * 《[C# 7.0 in a Nutshell][1]》
 * 《[Effective C#][39]》
 * 《[Inside Microsoft .Net Il Assembler][7]》
 * 《[Essential .NET, Volume I: The Common Language Runtime][14]》

Anders Hejlsberg 很少写书，《[The C# Programming Language][27]》大约是他认真写过的一本。我很喜欢看语言作者的书，比如K&R的《The C Programming Language》。只有语言作者，才是最了解这门语言的人。这本书里面最有价值的，是每一个语法，会有一段设计者的小旁白，可以更好的理解语言特性的设计哲学。

《[C# 7.0 in a Nutshell][1]》从全面的角度来说，比《[The C# Programming Language][27]》更适合入门，除了讲解语言本身之外，讲解了 .NET Framework 中的各种库。

《[Effective C#][39]》相当于《Effective C++》。

[Don Box][40] 的这本《[Essential .NET][14]》写得一般，远没有他的《[Essential COM][17]》出名。和《[Essential COM][17]》对标的书，是《[CLR via C#][3]》。


## 提高篇

 * 《[Depth in C#][2]》，马上出 第四版 了。更新到 C# 7.0。坐等 z.cn 给我送货。
 * 《[CLR via C#][3]》
 * 《[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries][13]》
 * 《[Customizing the Microsoft .NET Framework Common Language Runtime][15]》
 * 《[Concurrent Programming on Windows][16]》
 * 《[Writing High-Performance .NET Code][36]》

《[Depth in C#][2]》作者 Jon Skeet 是个奇葩。明明是在 [Google 工作][19]，吃着火锅写着 Java，但业余时间居然在玩 C#，一不小心，写了本最流行的 C# 书，成为了 Stack Overstack 上的 C# 最佳答主。Orz~

Jeffrey Richter 是个大骗子，最近几年写的书，每次序言都说"这是最后一次写书了"，我很高兴，以为这是最后一次掏钱了。然后没过几年，就给你出本新的。《[CLR via C#][3]》书名和《[Windows via C/C++][21]》一样，很具有欺骗性，如果你以为这两本书很容易，那就上当了。Jeffrey Richter 早年写书，同一本书，不同版本，名字还tm不一样，我下回再细细批判这事。

《[Customizing the Microsoft .NET Framework Common Language Runtime][15]》，按[赵姐夫][22]的说法，这本书比《[CLR via C#][3]》还艰深一些。在没有 .NET Core 的时代，我猜这本书就相当于《[Windows Internals][20]》，给你说了很多内部原理，就tm不给你看代码。让你云里雾里，隔空爽。嘿嘿~

《[Concurrent Programming on Windows][16]》书名也很唬人，它不教你API的。而是从 kernel 的线程模型，一路披荆斩棘，解读到 managed code 的线程模型。相当艰深的一本书，还没读完 =_=!。

《[Writing High-Performance .NET Code][36]》，发现一本挺新的书，看完再来评价。


## 设计模式篇

 * 《[Agile Principles, Patterns, and Practices in C#][41]》
 * 《[Adaptive Code: Agile coding with design patterns and SOLID principles][43]》

《[Agile Principles, Patterns, and Practices in C#][41]》是 [Uncle Bob(Robert C. Martin)][42] 的作品，从 OO 的设计原则（SRP、OCP、LSP 等等）讲起，再把所有设计模式（Factory、Composite、Observer 等等）全部用 C# 介绍一遍。

《[Adaptive Code: Agile coding with design patterns and SOLID principles][43]》和上一本差不多，只不过是最近出版的，比较新。没读完，先占坑 :-)

**面向对象的基本设计原则**

 * SRP，The Single-Responsibility Principle
 * OCP，The Open/Closed Principle
 * LSP，The Liskov Substitution Principle
 * DIP，The Dependency-Inversion Principle
 * ISP，The Interface Segregation Principle



## GC篇

 * 《[Garbage Collection][38]》
 * 《[The Garbage Collection Handbook][6]》

作者 [Richard Jones][37] 已经研究了 20+ years 的 GC 了。Orz~ 大佬~

唯一和 GC 相关的两本书，都是他写的。犀利~

据说没看过《[The Garbage Collection Handbook][6]》，不能说你懂 GC。嗯，我还没看过。


## 实践篇

### .NET Core

CLR & .NET Framework 几年前已开源，成为 .NET Core 或 dotnet core。

 * [.NET Core 文档][23]
 * [CoreCLR][24]，CLR 实现
 * [CoreFX][25]，.NET 标准库
 * [Roslyn][26]，C# compiler


### Shared Source CLI

Linus Torvalds 说过 "别BB，show me the code"。看完代码实现，才是真的懂了。

为了推广 .NET，Microsoft 在2001年推出了一份开源的.NET Framework实现，给高校作为学习、参考资料。史称 [Shared Source CLI][9]，实现了 C# 1.0，能同时跑在 Windows 和 FreeBSD 上。还有一本配套书籍《[Shared Source CLI Essentials][11]》。

2006年，更新了一版 SSCLI 2.0，实现了 C# 2.0，对标 .NET Framework 2.0。作者也更新了一版书籍《[Shared Source CLI 2.0 Internals][12]》，不过没出版。

之前 build 过一版本的 SSCLI 2.0，一直没时间读。

 * [https://github.com/kasicass/sscli20][8]

资料

 * [A Beginner's Guide to Microsoft's shared Source CLI (Rotor)][10]
 * 《[Shared Source CLI Essentials][11]》
 * 《[Shared Source CLI 2.0 Internals][12]》


### Mono

Miguel de Icaza 和 Mono，也是传奇故事。有空单独写一篇。

 * [https://github.com/mono/mono][35]


### ILRuntime

随着 Unity3D 的流行，在 Mono 上的热更新需求越来越强烈。为了一个项目统一一种开发语言(C#)，[蓝色幻想(蓝大)][34]发明了 ILRuntime。ps. 程序死宅都喜欢卡通妹子头像 :-)

 * [https://github.com/Ourpalm/ILRuntime][33]


## GITHUB & BLOGS

 * Anders Hejlsberg，[https://github.com/ahejlsberg][44]
 * Jon Skeet，[https://codeblog.jonskeet.uk/][18]
 * Miguel de Icaza，[https://tirania.org/blog/][32]
 

[1]:https://book.douban.com/subject/27177382/
[2]:https://www.amazon.com/CLR-via-4th-Developer-Reference/dp/0735667454
[3]:https://www.amazon.com/CLR-via-4th-Developer-Reference/dp/0735667454
[4]:https://github.com/kasicass/blog/blob/master/cpp/2018_11_23_farewell_cpp.md
[5]:https://music.163.com/#/song?id=34834414&autoplay=true
[6]:https://book.douban.com/subject/6809987/
[7]:https://www.amazon.com/Inside-Microsoft-Assembler-Serge-Lidin/dp/0735615470/
[8]:https://github.com/kasicass/sscli20
[9]:https://en.wikipedia.org/wiki/Shared_Source_Common_Language_Infrastructure
[10]:https://www.c-sharpcorner.com/article/a-beginners-guide-to-microsoft-shared-source-cli-rotor/
[11]:https://www.amazon.com/Shared-Source-Essentials-David-Stutz/dp/059600351X/
[12]:http://www.newardassociates.com/files/SSCLI2.pdf
[13]:https://www.amazon.com/Framework-Design-Guidelines-Conventions-Libraries/dp/0321545613
[14]:https://www.amazon.com/gp/product/0201734117/
[15]:https://www.amazon.com/Customizing-Microsoft®-Framework-Developer-Reference/dp/0735619883/
[16]:https://www.amazon.com/Concurrent-Programming-Windows-Joe-Duffy/dp/032143482X/
[17]:https://www.amazon.com/Essential-COM-Don-Box/dp/0201634465/
[18]:https://codeblog.jonskeet.uk/
[19]:http://askjonskeet.com/about
[20]:https://www.amazon.com/Windows-Internals-Part-architecture-management/dp/0735684189/
[21]:https://www.amazon.com/Windows-via-Jeffrey-M-Richter/dp/0735624240/
[22]:http://blog.zhaojie.me/
[23]:https://docs.microsoft.com/zh-cn/dotnet/core/
[24]:https://github.com/dotnet/coreclr
[25]:https://github.com/dotnet/corefx
[26]:https://github.com/dotnet/roslyn
[27]:https://www.amazon.com/Programming-Language-Covering-Microsoft-Development/dp/0321741765/
[28]:https://en.wikipedia.org/wiki/Anders_Hejlsberg
[29]:https://en.wikipedia.org/wiki/Category:Microsoft_technical_fellows
[30]:https://book.douban.com/subject/1106304/
[31]:https://book.douban.com/subject/1146437/
[32]:https://tirania.org/blog/
[33]:https://github.com/Ourpalm/ILRuntime
[34]:https://github.com/liiir1985
[35]:https://github.com/mono/mono
[36]:https://www.amazon.com/Writing-High-Performance-NET-Code-Watson/dp/0990583457/
[37]:https://www.cs.kent.ac.uk/people/staff/rej/gc.html
[38]:https://www.amazon.com/Garbage-Collection-Algorithms-Management-1996-08-16/dp/B01JXRNMDW/
[39]:https://www.amazon.com/Effective-Covers-Content-Update-Program/dp/0672337878/
[40]:https://en.wikipedia.org/wiki/Don_Box
[41]:https://www.amazon.com/Agile-Principles-Patterns-Practices-C/dp/0131857258/
[42]:https://en.wikipedia.org/wiki/Robert_C._Martin
[43]:https://www.amazon.com/Adaptive-Code-principles-Developer-Practices/dp/1509302581/
[44]:https://github.com/ahejlsberg
