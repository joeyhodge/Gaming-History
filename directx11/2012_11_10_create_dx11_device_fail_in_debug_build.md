# D3D11_CREATE_DEVICE_DEBUG 的坑

我在Win8下装了 VS2010 + DXSDK 来玩 dx11。发现跑官方的 tutorials 老失败。

设备创建就失败，这就诡异了。通过 dxdiag 可以看到，我的显卡虽然老了，但 dx11 通过 feature level 是支持老显卡的。

我显卡的 Feature Levels: 10.0,9.3,9.2,9.1

那为啥 D3D11CreateDeviceAndSwapChain() 会失败呢？突然发现自己是用 Debug build 的，然后换 Release build，搞定。

其中 Debug build 多了这么一句：

```C++
#ifdef _DEBUG
    CreateDeviceFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif
```

然后看看MSDN，被坑了。。。D3D11_CREATE_DEVICE_DEBUG 需要安装 Windows 8 SDK。

```
D3D11_CREATE_DEVICE_DEBUG

Creates a device that supports the debug layer. 
To use this flag, you must have D3D11*SDKLayers.dll installed; otherwise, device creation fails. To get D3D11_1SDKLayers.dll, install the SDK for Windows 8.
```

如果装的 VS2012，默认装 Windows 8 SDK，就没这个问题了。
 
 * [关于 dx11 的 Feature Level][1]
 * [D3D11_CREATE_DEVICE_FLAG enumeration][2]

2015.08.06

 * 发现 win10 + vs2012 也会出这个问题~


[1]:http://msdn.microsoft.com/en-us/library/windows/desktop/ff476872(v=vs.85).aspx
[2]:http://msdn.microsoft.com/en-us/library/windows/desktop/ff476107(v=vs.85).aspx

