# Enabling Direct3D Debug Information (Direct3D 9)

## D3D_DEBUG_INFO

 * 控制面板启用 d3d9 debug runtime
 * 利用 D3D_DEBUG_INFO，开启 debug information
 * WinXP、Win7 可用，Win8、Win10 不再支持 d3d9 debug runtime

```C++
#define D3D_DEBUG_INFO
#include <d3d9.h>
```

 * 可以获得很多信息
 * 使用 CreationCallStack 需要开启 "\\HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Direct3D\\D3D9Debugging\\EnableCreationStack"

```C++
DECLARE_INTERFACE_(IDirect3DCubeTexture9, IDirect3DBaseTexture9)
{
    ...

    #ifdef D3D_DEBUG_INFO
    LPCWSTR Name;
    UINT Width;
    UINT Height;
    UINT Levels;
    DWORD Usage;
    D3DFORMAT Format;
    D3DPOOL Pool;
    DWORD Priority;
    DWORD LOD;
    D3DTEXTUREFILTERTYPE FilterType;
    UINT LockCount;
    LPCWSTR CreationCallStack;
    #endif
};
```


## 参考资料

 * [Enabling Direct3D Debug Information (Direct3D 9)][1]

[1]:https://docs.microsoft.com/en-us/windows/win32/direct3d9/enabling-direct3d-debug-information
