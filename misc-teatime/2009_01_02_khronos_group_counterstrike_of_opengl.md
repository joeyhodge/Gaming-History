# Khronos Group - Counterstrike of OpenGL

自从 M$ 的 DirectX 系列不断发展、完善，凭借 windows 操作系统的统治地位，各大多媒体硬软件生产厂商只得俯首称臣，跟在 M$ 屁股后面，耗资耗力、马不停蹄地实现 M$ 陛下发布的无穷无尽的 specification。

3D图形标准上，Direct3D + HLSL，shader 得到硬件厂商的支持，弄得老大哥 OpenGL 也不停地更新，GLSL 也不甘落后，赶紧出马。最近 OpenGL 3.0 也快出来了，听说从上到下，彻底翻新了一下。当然，有竞争总是好的，垄断总使人不思进取。

不过成天跟着 M$ 跑确实让各大厂商头痛不已。因此  Apple, Sun, Intel, AMD(ATI), nVidia 等老大哥跑出来商议“我们弄个组织，把 multimedia 相关的标准统一统一，互相之间支持配合，免得给 M$ 拖垮了”，Khronos Group 应运而生。

 * http://en.wikipedia.org/wiki/Khronos_Group
 * http://www.khronos.org/about/

看看 Khronos 的组成，知名的硬件、软件、游戏公司都到齐了，不过左看右看，没有发现 M$，嘿嘿。M$ 自然是不能加入的，否则苦心打造的 DirectX 就算废了，垄断地位也没了。再说凭借 windows 操作系统的垄断，DirectX 只要说个啥，其他公司还不是依旧得跟着跑。不过在 non-windows 系统的地带，Khronos 已经形成了合围之势，M$ 与 Khronos 各占各的山头，各赚各的 money。

再看看 Khronos 的所管辖的标准，图像、音频、视频、通用计算，全部收归山头。目前 Khronos 的两大基石：OpenGL & OpenCL。

31st July, 2006，SGI 正式将 OpenGL specification 的定制权转交给 Khronos，从而让 Khronos 正式挑起3D图形标准的大旗。

而 apple 提出的 OpenCL(computing lanauage) 则是希望提供统一接口，让你的程序在任何 xxPU(CPU, GPU, DSP...) 上跑起来，充分发挥处理器的并行计算能力。虽然 1.0 specification 刚刚出来，但来自 Intel, IBM, AMD, nVidia 等等主流厂商的支持，会让 OpenCL 一路凯歌高奏。

有两个比较有趣的地方。

 * 虽然 creative 也在 Khronos 里，但 OpenAL 还未进入 Khronos；而 OpenSL|ES 则是 Khronos 为 embeded system 打造的 audio standard，但却不见 OpenSL 的标准版。个人认为，等 creative 与 Khronos 谈好筹码后，OpenAL 也会并入 Khronos，否则 OpenAL 就会被新标准替代掉。
 * OpenMP 也是面向分布式计算的标准，但主要是面向 CPU 的，不过 OpenCL 却得到了 AMD(ATI) + nVidia 等显卡厂商的支持，不知道 OpenCL 会不会慢慢替代 OpenMP，当然想替代也没这么容易，因为 OpenMP 支持者里面有 M$ 大爷，:-)~
