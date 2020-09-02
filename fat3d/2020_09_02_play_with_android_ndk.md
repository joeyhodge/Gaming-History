# Play with Android-NDK

很久没有 build android apk 了，最新的 Android SDK 从 [ant][6] 迁移到了 [gradle][5]。


## NDK 的安装

通过 Android Studio 的 SDK Manager，统一安装。（AndroidSdk 路径可以自己修改）

![](images/2020_09_02_play_with_android_ndk/sdk_manager.png)


## 配置 cmdline 环境

 * 假设 Android Studio 安装在："C:\Program Files\Android\Android Studio"
 * 假设 Android SDK 路径在："E:\AndroidSdk"
 * 配置一个 setup_build_env.cmd，可以方便调用 java & gradle

```
@echo off

set ANDROID_STUDIO_HOME="C:\Program Files\Android\Android Studio"
set JAVA_HOME=%ANDROID_STUDIO_HOME%\jre
set PATH=%PATH%;%JAVA_HOME%\bin
```


## Build ndk-samples

 * [https://github.com/android/ndk-samples][3]

### 关于 NDK (side by side)

 * SDK Manager 中可以选择安装不同版本的 NDK
 * 最新版的 NDK 在：AndroidSdk\ndk-bundle
 * 老版本的 NDK (side by side)，在：AndroidSdk\ndk\<version>

需要手工修改 local.properties，选择正确的 NDK 目录。比如：

```
ndk.dir=E\:\\AndroidSdk\\ndk\\21.2.6472646
sdk.dir=E\:\\AndroidSdk
```

### 编译

 * gradew.bat, 使用 [Gradle Wrapper][4] 编译项目
 * [Gradle Wrapper][4], 自动下载对应版本的 gradle，解决 buildtool 版本不一致问题
 * gradlew.bat tasks 可以看到哪些指令

```
cd project_dir
gradlew.bat assembleDebug
```


## Build "OpenGL ES 3.0 Programming Guide" Samples

 * 参考 ndk-samples，依样画葫芦，[Hello Triangle][1] 编译通过


## 后记

本想用 [emake][1] 来做 apk 生成的，不使用 cmake & gradle。[编译没问题][7]，设置好 cross-platform toolchain 即可。

android-armv7-debug.ini

```
[default]
flag=-O0,-g
home=e:/AndroidSdk/ndk-bundle/toolchains/llvm/prebuilt/windows-x86_64/bin
gcc=armv7a-linux-androideabi30-clang.cmd
ar=arm-linux-androideabi-ar.exe
as=arm-linux-androideabi-as.exe
name=android
```

00-Common.mak

```
mode: lib

out: ../EmakeBuild/Lib/Common.a
int: ../EmakeBuild/Temp/Common

flag: -Wall -Wformat-security
flag: -fPIC

inc: ../Common/Include
inc: E:\AndroidSdk\ndk-bundle\sources\android\native_app_glue

src: ../Common/Source/esShader.c
src: ../Common/Source/esShapes.c
src: ../Common/Source/esTransform.c
src: ../Common/Source/esUtil.c
src: ../Common/Source/Android/esUtil_Android.c
src: E:\AndroidSdk\ndk-bundle\sources\android\native_app_glue\android_native_app_glue.c
```

但生成 apk 还需要管理很多资源打包的功能，不用 gradle 看起来不大可能。有待继续研究。


[1]:https://github.com/kasicass/opengles3-book/tree/master/Chapter_2/Hello_Triangle/Android
[2]:https://developer.android.com/ndk
[3]:https://github.com/android/ndk-samples
[4]:https://docs.gradle.org/current/userguide/gradle_wrapper.html
[5]:https://gradle.org/
[6]:https://ant.apache.org/
[7]:https://github.com/kasicass/opengles3-book/tree/master/Emake
