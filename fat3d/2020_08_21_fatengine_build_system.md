# FATENGINE - Build System

## 设计思路

"Keep It Simple and Stupid"

 * Platform supports Windows/OpenBSD/Linux/Android
 * Using different C++ compilers make finding bugs easier
 * Windows - MSVC
 * OpenBSD - clang
 * Linux   - gcc

为何不使用 cmake 之类？

 * 手工维护 MSVC 和 emake 的工程文件并不复杂
 * 其实 cmake 很重，反而增加了复杂度


## Build Configuration

 * FAT_DEBUG_BUILD   - 关闭编译器优化，打开调试内容（比如：log、assert）
 * FAT_PROFILE_BUILD - 关闭编译器优化，打开调试内容
 * FAT_RELEASE_BUILD - 打开编译器优化，关闭调试内容


## Windows

 * 放在 Code/Solutions/VS2017 目录下
 * [games] - exe project
 * [libs]  - .lib/.dll project
 * [props] - props

```
├─FatEngine.sln
│
│-games
│   Game.vcxproj
│   Game.vcxproj.filters
│   Launcher.vcxproj
│   Launcher.vcxproj.filters
|   ...
|
├─libs
│   FatCommon.vcxproj
│   FatCommon.vcxproj.filters
│   FatInput.vcxproj
│   FatInput.vcxproj.filters
│   FatPlatform.vcxproj
│   FatPlatform.vcxproj.filters
│   FatRenderer.vcxproj
│   FatRenderer.vcxproj.filters
│   FatSystem.vcxproj
│   FatSystem.vcxproj.filters
|   ...
│
├─props
│   FatBase.props
│   FatLibraryOutput.props
│   FatLinkDirectX.props
│   FatMacroDebug.props
│   FatMacroProfile.props
│   FatMacroRelease.props
|   ...
```

### Props 的规划

 * FatBase.props, 所有 .lib / .dll / .exe 都需要它
 * FatLibraryOutput.props, .lib 输出到 /BinLib
 * FatMacroDebug/Profile/Release.props, 定义 FAT_DEBUG_BUILD / FAT_PROFILE_BUILD / FAT_RELEASE_BUILD
 * FatLinkDirectX.props，DirectX 相关的 .lib（比如：d3d9.lib, d3d11.lib, dinput.lib 等等）


## OpenBSD/Linux/Android

 * 所有 posix 环境，统一用 [emake][1] 管理

```shell
$ cd Code/Solutions/Emake
$ ./build.sh <debug|profile|release>
```

 * 放在 Code/Solutions/Emake 目录下

```
FatCommon.mak              emake makefile
FatPlatform.mak
...

linux_debug.ini            emake config
linux_profile.ini
linux_release.ini
openbsd_debug.ini
openbsd_profile.ini
openbsd_release.ini
```

 * build.sh 的实现
 * 根据操作系统和参数，生成 BUILD_INI（比如：openbsd_release.ini），然后丢给 emake

```bash
OSNAME=`uname | tr '[:upper:]' '[:lower:]'`
BUILD_CONF=`echo $1 | tr '[:upper:]' '[:lower:]'`

if [ "$BUILD_CONF" == "debug" ] || [ "$BUILD_CONF" == "profile" ] || [ "$BUILD_CONF" == "release" ] ; then
	BUILD_INI="${OSNAME}_${BUILD_CONF}.ini"
else
	echo "Usage:"
	echo "  build.sh <debug|profile|release>"
	exit
fi

emake --ini=$BUILD_INI zlib.mak 
emake --ini=$BUILD_INI FatCommon.mak
emake --ini=$BUILD_INI FatPlatform.mak
emake --ini=$BUILD_INI UnitTest.mak
```


## Reference

 * [emake][1]
 * [C/C++项目构建 (CMAKE & EMAKE)][2]


[1]:https://github.com/skywind3000/emake
[2]:https://github.com/kasicass/blog/blob/master/cpp/2018_11_07_cmake_and_emake.md
