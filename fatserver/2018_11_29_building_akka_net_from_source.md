# Building Akka.NET from Source

```
$ wget https://github.com/akkadotnet/akka.net/archive/v1.3.10.tar.gz
$ tar zxf v1.3.10.tar.gz
$ cd akka.net-1.3.10

$ ln -s /usr/bin/dotnet .dotnet/dotnet 
# aptitude install mono-runtime

$ ./build.sh
```

提示找不到 System.Core.dll，System.ComponentModel.Composition.dll：

```
Unhandled Exception:
System.IO.FileNotFoundException: Could not load file or assembly or one of its dependencies.
File name: 'System.Core, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'
  at NuGet.CommandLine.Program.Main (System.String[] args) [0x00005] in <d9d78a62f0a84697a113270ebd32594f>:0 
[ERROR] FATAL UNHANDLED EXCEPTION: System.IO.FileNotFoundException: Could not load file or assembly or one of its dependencies.
File name: 'System.Core, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'
  at NuGet.CommandLine.Program.Main (System.String[] args) [0x00005] in <d9d78a62f0a84697a113270ebd32594f>:0
```

装呗。


```
# aptitude install libmono-system-runtime4.0-cil libmono-system-componentmodel-composition4.0-cil


$ ./build.sh
```

又提示没有

 * Microsoft.Build.Utilities.dll
 * WindowsBase.dll
 * System.Net.Http.dll

装装装。实在搞不懂 debian9 的 package 怎么设计的。

```
# aptitude install libmono-microsoft-build4.0-cil
# aptitude install libmono-microsoft-build-utilities-v4.0-4.0-cil
# aptitude install libmono-windowsbase4.0-cil
# aptitude install libmono-system-net-http4.0-cil

$ ./build.sh
```

最后还是，失败了。

```
Could not load type 'NuGet.CommandLine.ConsoleProjectContext' from assembly 'NuGet, Version=4.3.0.6, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.
System.TypeLoadException: Could not load type 'NuGet.CommandLine.ConsoleProjectContext' from assembly 'NuGet, Version=4.3.0.6, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.
  at (wrapper managed-to-native) System.RuntimeType:GetPropertiesByName (System.RuntimeType,string,System.Reflection.BindingFlags,bool,System.Type)
  at System.RuntimeType.GetPropertyCandidates (System.String name, System.Reflection.BindingFlags bindingAttr, System.Type[] types, System.Boolean allowPrefixLookup) [0x00010] in <8f2c484307284b51944a1a13a14c0266>:0 
  at System.RuntimeType.GetProperties (System.Reflection.BindingFlags bindingAttr) [0x00000] in <8f2c484307284b51944a1a13a14c0266>:0 
  at System.ComponentModel.Composition.AttributedModel.AttributedPartCreationInfo+<GetExportMembers>c__Iterator0.MoveNext () [0x00110] in <0007e672dd7f4959adc6f8103d9c843f>:0 
  at System.Linq.Enumerable.Any[TSource] (System.Collections.Generic.IEnumerable`1[T] source) [0x00018] in <63992662b765477a898ef49cdcc99ee2>:0 
  at System.ComponentModel.Composition.AttributedModel.AttributedPartCreationInfo.HasExports () [0x0000c] in <0007e672dd7f4959adc6f8103d9c843f>:0 
  at System.ComponentModel.Composition.AttributedModel.AttributedPartCreationInfo.IsPartDiscoverable () [0x0001d] in <0007e672dd7f4959adc6f8103d9c843f>:0 
  at System.ComponentModel.Composition.AttributedModel.AttributedModelDiscovery.CreatePartDefinitionIfDiscoverable (System.Type type, System.ComponentModel.Composition.Primitives.ICompositionElement origin) [0x0000a] in <0007e672dd7f4959adc6f8103d9c843f>:0 
  at System.ComponentModel.Composition.Hosting.TypeCatalog.get_PartsInternal () [0x00052] in <0007e672dd7f4959adc6f8103d9c843f>:0 
  at System.ComponentModel.Composition.Hosting.TypeCatalog.CreateIndex () [0x0000b] in <0007e672dd7f4959adc6f8103d9c843f>:0 
  at System.Lazy`1[T].CreateValue () [0x00075] in <8f2c484307284b51944a1a13a14c0266>:0 

```

不死心，看[这里][1]，查一下 .exe 所有 reference 的加载策略。

```
$ export MONO_LOG_LEVEL=debug
$ export MONO_LOG_MASK=asm
$ mono --debug tools/nuget.exe

Mono: The following assembly referenced from /home/kasicass/myprj/akka.net-1.3.10/tools/nuget.exe could not be loaded:
    Assembly:   System.Xml.Linq    (assemblyref_index=4)
    Version:    4.0.0.0
    Public Key: b77a5c561934e089
```

少了 System.Xml.Linq.dll。少啥装啥，反复几次。

```
# aptitude install libmono-system-xml-linq4.0-cil
# aptitude install libmono-system-io-compression4.0-cil
# aptitude install libmono-system-data-services-client4.0-cil
# aptitude install libmono-system-servicemodel4.0a-cil
# aptitude install libmono-microsoft-csharp4.0-cil
# aptitude install libmono-system-runtime4.0-cil
```

终于能看到 nuget.exe 跑起来了。

有些 .dll，装了 package 也找不到，比如：

```
# aptitude install libmono-system-runtime4.0-cil
```

有些就没有对应的 package。全部从 dotnet core 里面复制呗


```
$ cp /usr/share/dotnet/sdk/NuGetFallbackFolder/system.runtime/4.1.0/lib/net462/System.Runtime.dll ./
$ cp /usr/share/dotnet/sdk/NuGetFallbackFolder/system.linq/4.1.0/lib/net463/System.Linq.dll ./
$ cp /usr/share/dotnet/sdk/NuGetFallbackFolder/system.reflection/4.1.0/lib/net462/System.Reflection.dll ./
$ cp /usr/share/dotnet/sdk/NuGetFallbackFolder/system.linq.expressions/4.1.0/lib/net463/System.Linq.Expressions.dll ./
$ cp /usr/share/dotnet/sdk/NuGetFallbackFolder/system.threading.tasks/4.0.11/ref/netcore50/System.Threading.Tasks.dll ./
$ cp /usr/share/dotnet/sdk/NuGetFallbackFolder/system.io/4.1.0/lib/net462/System.IO.dll ./
$ cp /usr/share/dotnet/sdk/2.1.500/Microsoft/Microsoft.NET.Build.Extensions/net461/lib/System.Net.Requests.dll ./
$ cp /usr/share/dotnet/sdk/2.1.500/Microsoft/Microsoft.NET.Build.Extensions/net461/lib/System.Collections.dll ./
$ cp /usr/share/dotnet/sdk/2.1.500/Microsoft/Microsoft.NET.Build.Extensions/net461/lib/System.Runtime.Numerics.dll ./
$ cp /usr/share/dotnet/sdk/2.1.500/Microsoft/Microsoft.NET.Build.Extensions/net461/lib/System.Threading.dll ./
```

最后跑到：

```
$ ./build.sh

Build failed.
Error:
Error while creating a fsi session.
InnerException:
The assembly for default symbol writer cannot be loaded
```

突然想起来，可以 [nuget 安装][2]。放弃折腾 build from source。

[1]:https://stackoverflow.com/questions/10872000/could-not-load-type-from-assembly-in-mono
[2]:https://www.nuget.org/packages/Akka/
