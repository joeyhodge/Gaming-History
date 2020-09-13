# Android与Linux内核的对应关系

## Android各版本和Linux版本对应关系

 * [Which Android runs which Linux kernel?][1]

```
 Android Version    |API Level  |Linux Version in AOSP                    |Header Version
----------------------------------------------------------------------------------------
 1.5   Cupcake      |3          |(2.6.27)                                 |
 1.6   Donut        |4          |(2.6.29)                                 |2.6.18
 2.0/1 Eclair       |5-7        |(2.6.29)                                 |2.6.18
 2.2.x Froyo        |8          |(2.6.32)                                 |2.6.18
 2.3.x Gingerbread  |9, 10      |(2.6.35)                                 |2.6.18
 3.x.x Honeycomb    |11-13      |(2.6.36)                                 |2.6.18
 4.0.x Ice Cream San|14, 15     |(3.0.1)                                  |2.6.18
 4.1.x Jelly Bean   |16         |(3.0.31)                                 |2.6.18
 4.2.x Jelly Bean   |17         |(3.4.0)                                  |2.6.18
 4.3   Jelly Bean   |18         |(3.4.39)                                 |2.6.18
 4.4   Kit Kat      |19, 20     |(3.10)                                   |2.6.18
 5.x   Lollipop     |21, 22     |(3.16.1)                                 |3.14.0
 6.0   Marshmallow  |23         |(3.18.10)                                |3.18.10
 7.0   Nougat       |24         | 3.18.48 4.4.0                           |4.4.1
 7.1   Nougat       |25         | ?                                       |4.4.1
 8.0   Oreo         |26         | 3.18.72 4.4.83  4.9.44                  |4.10.0
 8.1   Oreo         |27         | 3.18.70 4.4.88  4.9.56                  |4.10.0
 9.0   Pie          |28         |         4.4.146 4.9.118 4.14.61         |4.15.0
10.0   Q            |29         |                 4.9.191 4.14.142 4.19.71|5.0.3
```


## Android各个大版本主要解决问题

```
 Version | Description
----------------------------------------------------------------------------------------
 4.1     | Project Butter 为了让Android系统摆脱UI交互上的严重滞后感，希望能像“黄油”一样顺滑
 4.4     | Project Svelte，力求降低安卓系统的内存使用，解决低端机型升级难的问题
 5.0     | Project Volta，力求提升续航能力
 6.0     | 引入新的运行时权限，让用户能够更好地了解和控制权限
 7.0     | 引入新的JIT编译器(ART)，对AOT编译器的补充，可节省存储空间和加快更新速度
 8.0     | Project Treble，重新架构Android，将安卓系统框架与Vendor层解耦，力求彻底解决安卓碎片化这一老大难的问题。
 9.0     | 引入神经网络API，采用机器学习的思路来预测用户使用习惯来做省电优化，继续强化Treble计划
10.0     | Project Mainline，相关模块（Modules）不允许厂商直接修改，只能由Google应用商店来更新升级，强化用户隐私、系统安全与兼容性。支持脸部生物识别
```


## 模拟器中使用的几款手机

Nexus是Google的品牌，产品由Google设计，代工的厂家有HTC、Samsung、华硕和LG。

历代Nexus产品中，代工情况如下：
```
 Product      | OEM(original equipment manufacturer)
-----------------------------------------------------------------
 Nexus One    | HTC
 Nexus S      | Samsung
 Galaxy Nexus | Samsung
 Nexus 7      | Asus
 Nexus 10     | Samsung
 Nexus 4      | LG
 Nexus 5      | LG
```

[1]:https://android.stackexchange.com/questions/51651/which-android-runs-which-linux-kernel
