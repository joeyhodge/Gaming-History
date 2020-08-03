# OpenIndiana 2018.10 Crash Course


## 安装系统

下载 ISO，图形化安装，很简单。

* [http://dlc.openindiana.org/isos/hipster/latest/OI-hipster-text-20181023.iso][1]

See Solaris Runs~

![](2018_11_01_openindiana2018_crash_course_image_02.png)


## 设置 pkg publisher

默认装好的系统，DNS 是失效的，虽然 /etc/resolve.conf 设置ok的。有待研究。

```
$ cat /etc/resolv.conf 
nameserver  202.96.128.86
nameserver  202.96.134.33

$ ping www.163.com
ping: unknown host www.163.com
```

参见：[https://www.openindiana.org/packages/][2]

```
# pkg publisher
PUBLISHER                   TYPE     STATUS P LOCATION
openindiana.org             origin   online F http://pkg.openindiana.org/hipster/
```

因为 DNS 问题，pkg 不工作了。改成使用IP。

```
# dig pkg.openindiana.org
...
;; ANSWER SECTION:
pkg.openindiana.org.    600     IN      A       91.194.74.133
...

# pkg unset-publisher openindiana.org
# pkg set-publisher -g http://91.194.74.133/hipster openindiana.org
```

It works。不过 OpenIndiana 的源实在是太少了，访问很慢。


## 软件安装

```
# pkg search python3
INDEX           ACTION VALUE                                         PACKAGE
...
basename        link   usr/bin/python3                               pkg:/runtime/python-34@3.4.9-2018.0.0.1
basename        link   usr/bin/python3                               pkg:/runtime/python-35@3.5.6-2018.0.0.0
...

# pkg install /runtime/python-35
```


## 常用软件

```
# pkg install runtime/python-35
# pkg install developer/gcc-8
```

## vim & tmux 的基本配置

```
$ cat ~/.vimrc
" basic
syn on
set tabstop=2
set nobackup
set background=dark
colorscheme desert
set number

$ cat ~/.tmux.conf
# hjkl pane traversal
bind h select-pan -L
bind j select-pan -D
bind k select-pan -U
bind l select-pan -R

# reload me
bind r source ~/.tmux.conf\; display "/.tmux.conf sourced!"
```

## 问题

* DNS的问题
* gcc 找不到 stdlib.h, stdio.h 这些基本库
* 玩不动，换一个 OpenSolaris 的发布版试试 -- [OmniOSce][3]


## 附录1：手工配置网络

nwam 是自动配置网络的服务。关闭之，切换到手动配置。（关闭 nwam，现有网卡就全部不工作了）

```
# svcadm disable network/physical:nwam
# svcadm enable network/physical:default
```

看看有哪些网卡

```
# dladm show-link
LINK       CLASS        MTU      STATE      BRIDGE      OVER
e1000g0    phys         1500     unknown    --          --
```

给网卡挂接 ip interface

```
# ifconfig e1000g0 plumb
```

这时候我们可以用 ifconfig 看到此网卡了

```
# ifconfig e1000g0
e1000g0: flags=1000842<BROADCAST,RUNNING,MULTICAST,IPv4> mtu 1500 index 4
        inet 0.0.0.0 netmask 0
        ether 8:0:27:6:b3:29
```

```
# ifconfig e1000g0 dhcp start
... bla bla ...

# ifconfig e1000g0
e1000g0: flags=1000842<BROADCAST,RUNNING,MULTICAST,IPv4> mtu 1500 index 4
        inet 192.168.0.117 netmask ffffff00 broadcast 192.168.0.255
        ether 8:0:27:6:b3:29
```

在 network/physical:default 模式下，需要创建两个空文件，让系统在下次重启时，自动 plumb + dhcp。

```
# touch /etc/hostname.e1000g0 /etc/dhcp.e1000g0
```


[1]:http://dlc.openindiana.org/isos/hipster/latest/OI-hipster-text-20181023.iso
[2]:https://www.openindiana.org/packages/
[3]:https://omniosce.org/
