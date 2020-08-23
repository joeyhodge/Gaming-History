# Visual Studio 中找不到 cl.exe

安装了 Vistual Studio 2019，但 x86 版本总是 build 失败，而 x64 则没问题。

```
TRACER can't find 'cl.exe'
error MSB6006: "CL.exe" exited with code -5
```

后来发现是修改过这个 .props

```
C:\Users\{username}\AppData\Local\Microsoft\MSBuild\v4.0Microsoft.Cpp.Win32.user.props
```

发现个好方法：

 * [在VS中看MSBuild的详细输出][1]：Tools –> Options –> Projects and Solutions –> Build and Run 

[1]:https://dailydotnettips.com/back-to-basic-displaying-detailed-output-of-msbuild-in-visual-studio-output-window/
