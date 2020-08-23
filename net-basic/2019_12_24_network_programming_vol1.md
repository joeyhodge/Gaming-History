# UNPv1

哪天重读《UNPv1》，可以认真写写读书笔记

```
UNPv1 2nd v.s. 3nd

对比了一下 Stevens 老大的《UNIX Networking Programming vol.1》的 2nd 与 3nd，发现 XTI: X/Open Transport Interface 那一堆 APIs 消失了，取而代之的是 SCTP 的详尽介绍。

XTI 是由 SVR3 系统引入的一套 APIs，当年 BSD TCP/IP 与 OSI 两军大战，标准之争最后花落 TCP/IP，而 OSI 仅仅作为概念性网络模型，出现在教科书上。XTI 狂赌 OSI，可惜棋错一着，黯然退出历史舞台。下面摘录一段 wikipedia 中对 TLI(XTI) 的介绍。

Although TLI and STREAMS were first introduced with SVR3, no actual protocol implementations were provided in System V until SVR4 shipped with TCP/IP support. It was originally expected that the OSI protocols would supersede TCP/IP, thus TLI is designed from an OSI model-oriented viewpoint, corresponding to the OSI transport layer.

TLI and XTI were never as widely used as BSD sockets, and although they are still supported in SVR4-derived operating systems such as Solaris (as well as "classic" Mac OS, in the form of Open Transport), sockets are now the de facto standard networking API.

虽然退出舞台，但一套标准、一套接口的设计，对后来的影响默克估计，也是绝好的学习材料。可惜 UNPv1 3nd 将 XTI 的内容删节了，真希望能弄到一本 2nd。而 3nd 中取而代之的 SCTP，就目前来说，FreeBSD, Linux 的正式发布版中还是不支持的，需要自己打 patch。

ps. 传说中，Stevens 老大还有出《UNP vol.3》的计划，可惜他老人家驾鹤西去，甚憾！
```