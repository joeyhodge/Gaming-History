# [uC/OS-II] 代码编译

[原书][1] 提供的代码，使用 Borland Turbo C++ 4.5 编译。好古老，但我发现居然有[下载][2]。

可惜下回来的安装程序，DOSBox 无法直接安装。只能在 16-bit windows 下安装。这。。。

难道我还要在 virtualbox 里面装一次 [Windows 95][5]？还真有人[干过][6]。

想想，不对。这道路越走越偏了。无法编译也无所谓。读读代码就好。

等最后，这些代码总需要用 NASM 再写一遍。不如直接开始学 NASM 吧。

* [各种 develop tools for DOS][7]
* [NASM for DOS][8]
* [Open Watcom Compiler for DOS][9]

把 NASM 和 Open Watcom 整合到我的 [uC/OS-II sandbox][10] 中：

 * C: 放 NASM / Open Watcom 等工具。
 * D: 放 uC/OS-II 光盘内容
 * E: 放自己写的代码

dosbox.conf 改一改，启动注册好 NASM / Open Watcom 开发环境。

```
MOUNT C: .\C
MOUNT D: .\D
MOUNT E: .\E

SET NASMPATH=C:\DEVEL\NASM
SET PATH=%NASMPATH%;%PATH%

D:
C:\DEVEL\OW\OWSETENV.BAT
```

可以在 DOSBox 里面写程序了。

![](images/2018_11_18_compile_ucos2/owcc_runs.png)


**参考资料**

 * [编译 FreeDOS][3]
 * [FreeDOS 相关程序][4]

[1]:https://book.douban.com/subject/1229913/
[2]:https://winworldpc.com/product/turbo-c/45-win
[3]:http://zet.aluzina.org/index.php/Compiling_FreeDOS
[4]:http://www.freedos.org/software/?prog=pacific-c
[5]:https://winworldpc.com/product/microsoft-windows-boot-disk/95-osr2x
[6]:http://thecuriousgeek.org/2012/03/installing-windows-95-in-virtualbox/
[7]:http://www.ibiblio.org/pub/micro/pc-stuff/freedos/files/distributions/1.2/repos/pkg-html/group-devel.html
[8]:http://www.ibiblio.org/pub/micro/pc-stuff/freedos/files/distributions/1.2/repos/pkg-html/nasm.html
[9]:http://www.ibiblio.org/pub/micro/pc-stuff/freedos/files/distributions/1.2/repos/pkg-html/ow.html
[10]:https://github.com/kasicass/ucos
