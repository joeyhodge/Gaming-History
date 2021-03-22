# HLSL Compiler

* [Youtube Video][1]
* https://github.com/microsoft/DirectXShaderCompiler



## Why DXC?

### FXC - old compiler

* The old shader compiler is deprecated
  * Header: `d3dcompiler.h`
  * Lib: `d3dcompiler.lib`
  * Frontend: `D3DCompiler_47.dll`
  * Intermediate language: `DXBC`
  * Command line: `fxc.exe`
* Supported up to Shader Model 5.1
* Modern drivers convert DXBC to DXIL at runtime


### DXC - new compiler

* The new shader compiler
  * Header: `dxcapi.h`
  * Lib: `dxcompiler.lib`
  * Frontend: `dxcompiler.dll`
  * Intermediate language: `DXIL`
  * Command line: `dxc.exe`
* Supports Shader Model 6.0+
  * Wave intrinsics, 16 bit types, raytracing, variable rate shading, etc.


### DXC is on Github!

Issues lists (bugs)

* https://github.com/microsoft/DirectXShaderCompiler/issues

Documentation (wiki)

* https://github.com/microsoft/DirectXShaderCompiler/wiki
* Shader model details
* Interface and usage notes
* [FXC to DXC port guide][2]

Downloadable releases (faster updates than Windows SDK)

* https://github.com/microsoft/DirectXShaderCompiler/releases/


### Why the changes?

fxc had reached its limits

* Not designed to handle large numbers of large shaders
* Difficult to optimize further
* Difficult to expose console capabilities
* Long compoile times for some shaders

dxc is the path forward

* Based on industry standard Clang/LLVM
* Codebase is easier to modify and improve
* Developed by large team over several years
* Development remains active
* Supports SPIR-V
* Already in use by several major studios and on the Xbox family of Consoles



## Using dxc.exe and dxcompiler.dll

### DXC and dxcompiler.dll design goals

* DXC and dxcompiler.dll are used to build thousands of shaders for retail titles
* We recently updated dxc and the dxcompiler.dll interface (IDxcCompiler3).
  * Command line arguments are now fed directly to the dxcompiler.dll
    interface unifying functionality.
  * PDBs and Reflection data are now far easier to store **separately** from
    your compiled shader (vs. **embedded** in the shader object file).
  * For the interface, the various outputs (both separate and embeded) are
    available immediately after compile.


### Compilation Flow

![](images/2021_01_10_shader_system/dxc-compilation-flow.png)


### Recommended Usage

* Compile the shader source from myshader.hlsl and store the
  shader (-Fo) in myshader.dxo
* Generate debug info (-Zi) and store a separate .pdb file (-Fd)
  for debugging later
* Generate separate reflection (-Fre) and don't include it in
  the shader (-Qstrip_reflect)
  * Can be used at asset creation time and then thrown away
  * Results in smaller shader sizes which add up for many shaders

```
> dxc.exe myshader.hlsl
          -Fo myshader.dxo
          -Zi -Fd myshader.pdb
          -Fre "myreflection.ref" -Qstrip_reflect 
```



## Shader Symbols (PDBs)

TODO



[1]:https://www.youtube.com/watch?v=tyyKeTsdtmo
[2]:https://github.com/microsoft/DirectXShaderCompiler/wiki/Porting-shaders-from-FXC-to-DXC

