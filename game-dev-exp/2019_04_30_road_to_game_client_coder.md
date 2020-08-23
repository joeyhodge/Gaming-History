# 游戏客户端引擎程序员 - 打怪升级路线图

## 语言基础

C

 * C语言最佳入门，[https://learncodethehardway.org/c/][8]
 * 《[The C Programming Language][3]》
 * 《[Expert C Programming][4]》
 * 《[C Traps and Pitfalls][5]》
 * 《[C Interface and Implementation][6]》

C++

 * 如何学 C++，看 [这里][7]

Lua or Python

 * 搞懂一门脚本语言
 * 书只是辅助，推荐直接读代码
 * 《[Lua设计与实现][2]》
 * 《[Python源码分析][1]》


## 操作系统 + Linker & Loader

 * 《[Programming Windows 5th][9]》
 * 《[Multithreading Applications in Win32][11]》
 * 《[Windows Via C/C++ 5th][10]》
 * 《[程序员的自我修养--链接、装载与库][12]》

《[Programming Windows 5th][9]》只需要看前面几章，理解 Win32 消息循环即可。

《[Multithreading Applications in Win32][11]》，理解多线程是怎么回事；侯捷有个翻译版，翻得很好。


## GUI

 * 推荐 [WPF][13]
 * 虽然很多公司还在用 MFC


## 3D图形学

按着下面的练习做一遍，碰到不懂就看书。图形学是个填不完的坑~

 * [https://handmadehero.org/][14]


### 基础篇

 * 上篇，各种概念与技术。《Fundamentals of Computer Graphics 3rd》 or 《Computer Graphics - Principles and Practice》
 * 中篇，各种算法。《Real-Time Rendering 3rd》
 * 下篇，高阶追求。《Physically Based Rendering - From Theory To Implementation 2nd》


 * 杂篇1，只谈数学，《3D Math Primer for Graphics and Game Development 2nd》
 * 杂篇2，同上，《Mathematics for 3D Game Programming and Computer Graphics 3rd》


### API之术（DX9/11 or OpenGL）

 * 《3D绘图程序设计》，dx9/10/ogl 实现各种3D游戏需要的效果
 * 《More OpenGL Game Programming》，如何用 gl 实现各种中高级效果
 * 《Introduction to 3D Game Programming with DirectX 9.0c - A Shader Approach》，龙书
 * 《Introduction to 3D Game Programming with DirectX 11》，龙书 DX11 版
 * 《Practical Rendering and Computation with Direct3D 11》，DX11 宝典
 * 《OpenGL Programming Guide》，OpenGL 宝典
 * 《OpenGL Shading Language》，glsl 宝典


### 引擎设计

 * David Eberly 的《3D Game Engine Design》，WildMagic，这有一个完整的引擎实现
 * Jason Gregory 的《Game Engine Architecture》
 * 《Character Animation with Direct3D》，专注讲 model、animation 的。


### 各种技巧

 * ShaderX 系列
 * 《Real Time Collision Detection》
 * 《Real Time Cameras》
 * 《Real-Time Shadows》


## 代码

Engine Code

 * [DOOM3 Source Review][16]
 * [Quake3 Source Review][17]

Collision Detection

 * [OPCODE][15]


[1]:https://book.douban.com/subject/3117898/
[2]:https://book.douban.com/subject/27108476/
[3]:https://www.amazon.com/Programming-Language-2nd-Brian-Kernighan/dp/0131103628/
[4]:https://www.amazon.com/Expert-Programming-Peter-van-Linden/dp/0131774298/
[5]:https://www.amazon.com/C-Traps-Pitfalls-Andrew-Koenig/dp/0201179288/
[6]:https://www.amazon.com/Interfaces-Implementations-Techniques-Creating-Reusable/dp/0201498413/
[7]:https://github.com/kasicass/blog/blob/master/cpp/2018_11_23_farewell_cpp.md
[8]:https://learncodethehardway.org/c/
[9]:https://www.amazon.com/Programming-Windows®-Fifth-Developer-Reference/dp/157231995X/
[10]:https://www.amazon.com/Windows-via-Jeffrey-M-Richter/dp/0735624240/
[11]:https://www.amazon.com/Jim-Beveridge-Multithreading-Applications-Win32/dp/B00N4IBAI6/
[12]:https://book.douban.com/subject/3652388/
[13]:https://en.wikipedia.org/wiki/Windows_Presentation_Foundation
[14]:https://handmadehero.org/
[15]:http://www.codercorner.com/Opcode.htm
[16]:http://fabiensanglard.net/doom3/index.php
[17]:http://fabiensanglard.net/quake3/index.php
