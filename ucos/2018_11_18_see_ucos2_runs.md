# [uC/OS-II] See uC/OS-II runs again~


## Run~ Run~

[《嵌入式实时操作系统uC/OS-II(第二版)》][1] 翻译得很不错，就没必要读英文版了。

在 win10 上，把附属光盘中的内容拿来一跑，WTF，居然提示此程序不支持 64-bit系统。

 * 跑 ex1_x86 的例子
 * \SOFTWARE\uCOS-II\EX1_x86L\BC45\TEST\TEST.exe

这年头，PC / Laptop 都是 64-bit系统 了，想找个 32-bit 的 windows 来跑例子还真不容易。难道要在 VirtualBox 上装个 32-bit windows？

突然想到了 [DOSBox][2]。使用版本 DOSBox-0.74-2。

修改配置文件 dosbox.conf，在最后加上如下内容，让 DOSBox 可以读到光盘的内容。

```
[autoexec]
MOUNT D: .\ucos2
D:
```

启动 DOSBox，试试

```
> cd \SOFTWARE\uCOS-II\EX1_x86L\BC45\TEST
> TEST.exe
```

Woo! See uC/OS-II runs again~

![](images/2018_11_19_ucos2_examples/example01.png)

把光盘内容和 dosbox.conf 打包放在 [github 上][3]。备查。


## 为何要选用 uC/OS-II？

现代操作系统（Linux、FreeBSD）的代码，发展了这么多年，已经很复杂了。但"硬件 + 软件"最基本的那些概念，并没有什么变化。

从一个嵌入式操作系统入手，可以更容易理清思绪，避开不必要的内容。

 * uC/OS-II 本科时候读过，也可以更快上手。
 * 这么多年的发展，uC/OS 的代码千锤百炼，经过了时间的检验。


[1]:https://book.douban.com/subject/1229913/
[2]:https://www.dosbox.com/
[3]:https://github.com/kasicass/ucos
