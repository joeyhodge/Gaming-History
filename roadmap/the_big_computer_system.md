# THE BIG COMPUTER SYSTEM - A GAME CODER'S VIEW

游戏程序员眼中的计算机体系

```
+-------------------------------------------------------------------------------+
|                    Software Architecture Principle(L10)                       |
+-------------------------------------------------------------------------------+

+-------------------------------------------------------+        +--------------+
|             App / Game / Game Engine(L8)              |        |              |
+------------+------------------------+-----------------+ <====> |              |
|   GUI(L4)  |   DB(L5)  |  Network(L6)  | 3D API (L7)  |        | Programming  |
+------------+------------------------+-----------------+ <====> |   Languages  |
|      System API(L2)       |   Compiler & Linker(L3)   |        |     (L9)     |
+-------------------------------------------------------+ <====> |              |
|               Hardware/Operating System(L1)           |        |              |
+-------------------------------------------------------+        +--------------+
```


## Road To the Kernel (L1/L2)

 * CPU Architecture
 * Embedded System Development
 * Kernel Developer
 * Windows / Android / OpenBSD / Debian
 * [Roadmap][1]


## Compiler & Linker & Debugger (L3)

 * clang / llvm
 * [fatcc], a experimental C compiler implementation
 * [fatjvm], a experimental Java VM implementation
 * [fatclr], a experimental CLR & C# implementation


## GUI (L4)

 * Desktop / Mobile GUI framework
 * [WPF][10]
 * [fatwpf], a cross-platform WPF implementation


## Database Demystify (L5)

 * data storage layer
 * database fundamental, e.g. transaction
 * sqlite
 * mysql
 * mongodb


## Network (L6)

 * TCP/IP stack
 * libuv
 * zmq, [ZeroMQ Demystify][9]


## 3D API (L7)

 * DirectX 9 / 11 / 12
 * OpenGL / OpenGL ES
 * Vulkan


## App / Game / Game Engine (L8)

 * [fatdemo][12], a simple render framework for algorithm testing
 * [fatengine][11], a product-ready 3dengine
 * [fatserver][8], a distribute multi-process game server
 * [fatmonkey], a RenderMonkey implemtation using [fatwpf]
 * [fatterminal], a SecureCRT-like terminal manager


## Language Zoo (L3/L9)

 * Native: Assembly, C, [C++][13]
 * JIT: C#, Java
 * Scripting: Python / Lua / Pypy / Luajit


## Software Architecture Principle (L10)

重剑无锋，大巧不工。

 * [SAP01 - Actor Model][2]

参考资料

 * 《[The Architecture of Open Source Applications][7]》


## NOTE

**2020.09.09**

重构了整个技术路线图，操作系统加入 Android，随身的手机就是一个可以把玩的 Linux 系统。编程语言加入 Java，方便研究 Android。

《[Road To the Kernel][1]》有空再继续，将精力集中在游戏客户端技术上。将服务端技术作为业余项目，比如：[fatserver][8]、database，有空再研究。

游戏客户端路线图

 * 目标：所有依赖技术，自我打造
 * [fatengine][11], 用来做做小游戏、小demo
 * [fatwpf], 跨平台 WPF 实现，绑定到 .net core，用来写 GUI app（游戏编辑器、Profile工具等等）
 * [fatengine][11] 中的 in-game GUI 会用 xaml 来做布局，内嵌一个 [fatwpf-lite]

实验性质的项目，不知啥时候会启动

 * [fatcc], 一个简单的 c compiler, just for fun~
 * [fatjvm], 参考 android ART vm, just for fun~
 * [fatclr], 参考 Shared Source CLI, just for fun~
 * [fatmonkey], AMD 经典的 RenderMonkey
 * [fatterminal], 实现自己的 terminal manager, using WPF

**2018.11.26**

最近在填自己挖的坑《[Road To the Kernel][1]》，和顾QQ吹牛说：

```
从 CPU 到 OS，
从 assembler 到 compiler，
从 filesystem 到 database，
从 TCP/IP stack 到 game server，
最后再到 zookeeper/hadoop/spark。
全部撸一遍，打通整个计算机体系的任督二脉。
```

顾QQ不屑："你丫是个理想主义者。"

就理想一次吧。:-) 索性把"个人技术体系refactoring"的战线扩大。

《The Big Computer System》，我的计算机系统 hack 之路。Just for fun~


[1]:https://github.com/kasicass/blog/blob/master/minibook/road_to_the_kernel.md
[2]:https://github.com/kasicass/blog/blob/master/design-principle/2018_11_28_actor_model.md
[3]:https://www.unrealengine.com/
[4]:https://unity.com/
[5]:https://github.com/kasicass/blog/blob/master/game-dev/2019_04_30_road_to_game_client_coder.md
[6]:https://github.com/kasicass/blog/blob/master/unity3d/2019_05_01_road_to_unity3d_coder.md
[7]:http://aosabook.org/en/index.html
[8]:https://github.com/kasicass/blog/blob/master/fatserver/2018_11_29_fatserver_design.md
[9]:https://github.com/kasicass/blog/blob/master/minibook/zeromq_demystify.md
[10]:https://en.wikipedia.org/wiki/Windows_Presentation_Foundation
[11]:https://github.com/kasicass/blog/blob/master/minibook/fat3d_design_and_implementation.md
[12]:https://github.com/kasicass/fatdemo
[13]:https://github.com/kasicass/blog/blob/master/lang-cpp/2018_11_23_farewell_cpp.md
