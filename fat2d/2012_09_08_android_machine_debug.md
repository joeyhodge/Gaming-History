# Android App 真机测试

Android App 真机测试真是太方便了。

 * 设置 => 应用程序 => 开发 => USB调试
 * On the device, go to Settings > Applications > Development and enable USB debugging (on an Android 4.0 device, the setting is located in Settings > Developer options)

将手机插上 USB 线，在 Mac 下直接就好了。Windows/Linux 还需要配置下，参见：（win32/linux弱暴了 :-）

 * [http://developer.android.com/tools/device.html][1]

查看链接的 device

```
> adb devices
List of devices attached 
S5570220d114d device

# -d, 将程序安装到 device
> adb -d install bin/AndroidSDL-debug.app
```

搞定～

ps. 我针对 Android 4.1(android-16) 编译的 .apk，在我 Android 2.2 的机器上也能直接跑，不错，不错，赞一个。

[1]:http://developer.android.com/tools/device.html
