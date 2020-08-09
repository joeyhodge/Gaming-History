# OpenGL ES multi-threading

egl标准是有多线程支持的，但是小米居然不支持。

 * android 上可以启用多线程，但多线程访问 gles 函数是无效的。

PowerVR模拟器居然是生效的，但真机测试无效。

 * 模拟器生效，那是模拟器的 bug(参见PowerVR同学的回复): khronos 论坛一个链接，找不到了。

实际的硬件/驱动实现不支持 multi-threading，还是不要用了。

 * [http://www.khronos.org/message_boards/viewtopic.php?f=9&t=2254][1]

[1]:http://www.khronos.org/message_boards/viewtopic.php?f=9&t=2254
