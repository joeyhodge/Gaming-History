# 手机 app 开发实战 -- 点餐系统

[fatfatson][5] 做了套点餐系统（EPOS，Electronic Point-of-Sales）。整个技术选型很有参考价值，记录下。

这篇文章，会慢慢记录这套系统的所有技术细节。


## 业务需求

TODO


## 前端方案

考察过这些

 * [react native][1]
 * [xamarin][2]
 * [qt][3]
 * [cordova][4]

### cordova

cordova 是包了个浏览器，滚动不平滑，排除。

### qt

qt 那种闭门造车，好不容易引入 js 还自己搞方言的引擎，跟不上 js 主流社区，排除。

### xamarin

xamarin 别的都不错，但开发体验上，C# 和 js 有很大差别，在移动端特别明显。

 * C# 程序，你改一行代码，必须全部编译，重新打包安装。
 * js，可以直接在设备上热加载，这边一保存，那边就有效果。

其实现在 app 开发 流行的所谓热更，准确说，是冷更。把代码下载下来，编译，然后重新启动而已。真正的热更，是程序不退，直接加载、运行新代码，旧的状态数据还在。

### react native

react native 可以做到这一点，这就是 react+redux 的优势。

为了做到热更，需要从程序构建设计上来支持。react+redux 就是从设计上，逻辑重加载后，取回外置数据重新选择。

[fatfatson][5] 最后选了 rn。

rn 这一套，和我司游戏开发的体验，是一样的。:-)

### 其它

谈到前端跨平台方案，也讨论了下 C++ 热更。谈到了 UE4。

"UE4搞了套 C++ 的逻辑重加载，每个类要写一堆数据序列化的操作。这个绝对是走邪路。比3代的 UnrealScript 还要不靠谱。"

深以为然。但这不妨碍鹅厂用人力抹平开发效率上的问题。


## 后端方案

TODO

## 发布流程

TODO

 * gitlab
 * docker



[1]:https://facebook.github.io/react-native/
[2]:https://visualstudio.microsoft.com/xamarin/
[3]:https://www.qt.io/
[4]:https://cordova.apache.org/
[5]:https://fatfatson.github.io/
