# WPF Architecture

![](images/2020_09_11_wpf_architecture/architecture.png)


## Managed Layer

**Presentation Framework (PresentationFramework.dll)**

 * 用户使用的基本功能
 * controls, data bindings, styling, shapes, media, documents, annotations, animation

**Presentation Core (PresentationCore.dll)**

 * managed wrapper of MILCore
 * Visual System: Visual, VisualElement and rending instructions


## Unmanaged Layer （WpfGfx_version.dll)

 * MILCore (Media Integration Library Core)
 * a D3D9 renderer, efficient hardware and software rendering
 * Composition System: translates the instructions of Visual System to DirectX calls


## Core API Layer

 * Win32 系统 API


## Reference

 * [Github: WPF repo][2]
 * [MSDN: WPF Architecture][1]
 * [MSDN: Advanced WPF Areas][4]
 * [Understanding WPF Architecture][3]

[1]:https://docs.microsoft.com/en-us/dotnet/desktop/wpf/advanced/wpf-architecture
[2]:https://github.com/dotnet/wpf
[3]:https://www.dotnettricks.com/learn/wpf/understanding-wpf-architecture
[4]:https://docs.microsoft.com/en-us/dotnet/desktop/wpf/advanced/?view=netframeworkdesktop-4.8
