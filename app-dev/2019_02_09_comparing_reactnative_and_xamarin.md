# Comparing React Native & Xamarin


## React Native

全栈 TypeScript(JavaScript)

前端；React Native
后端：Node.js


## Xamarin

全栈 C#

前端：Xamarin + ILRuntime
后端：.NET Core (ASP.NET or AKKA.NET)


## fatfatson 的评价

xamarin就是为了统一语言，不仅仅是逻辑开发语言，连调用系统api所用的语言都统一了。在rn里如果要添加扩展功能，需要各平台用各自的原生语言去写一套，分别导出到js里用。

另外，rn透明性和可定制性比xamarin还是高一点，rn毕竟只是一个集成了js引擎的小框架，把原生api和控件封装一下导出到js里，干的事情都是很好理解的，要改js引擎，要添加扩展，都很容易做，但xamarin是整套mono，比起js大几个量级，而且强行把所有api映射到c#里，不是每个地方都那么自然，想对它做修改，基本上无能为力了。

目前来看，rn适合web团队转型做app，xamarin适合游戏团队转型做app，但前提都是要有个人能镇住场子，给写逻辑的人提供稳定环境

rn配置极其麻烦，随便一个小库升级都可能导致整个工程编不过，而且es语言发展太快，社区轮子多，很多东西没有统一标准，对新手来说，很容易无所适从，碰到问题的概率也很大

 * 在语言和工具链层面，xamarin优于rn，适合菜鸟用
 * 在hotreload和社区层面，rn优于xamarin，适合高手用

但是xamarin里就只要写c#，因为它已经把所有平台api都导出到c#了。如果要使用第三方原生库，就需要自己生成胶水代码，它也提供了工具做，在必要的时候（跨语言语法不兼容）也还要手写一些代码，不过都有现成的规范，差不多照着做就行。


[1]:https://facebook.github.io/react-native/
[2]:https://visualstudio.microsoft.com/xamarin/
