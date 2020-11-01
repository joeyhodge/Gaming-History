# Rendering Technique Index (渲染技术一览表)

 * 记录感兴趣的渲染技术


## Rendering Pipeline

 * Forward Rendering
 * Defered Lighting
 * Forward+ Rendering
 * GPU-Driven
 * FrameGraph
 * Stingray Renderer Walkthrough

### Forword Rendering

TODO

### Defered Lighting

TODO

### Forward+ Rendering

 * [2012 - Forward+: Bringing Deferred Lighting to the Next Level][1]
 * [Forward框架的逆袭：解析Forward+渲染][4]
 * [VFPR - a Vulkan Forward+ Renderer][2]
 * [AMD Forward+ Sample on DirectX 11][3]
 * [AMD - Leo Demo][26]

### GPU-Driven

 * [2019 - GDC - GPU-Driven Rendering and Virtual Texturing in Trials Rising][8]
 * 2017 - GPUZEN - Deferred+: Next-Gen Culling and Rendering for Dawn Engine
 * [2016 - GDC - Optimizing the Graphics Pipeline with Compute][9]
 * [2016 - GPUOPEN - GCN Shader Extensions for Direct3D and Vulkan][5]
 * [2015 - SIGGRAPH - GPU-Driven Rendering Pipelines][6]
 * [2008 - SIGGRAPH - GPU-Based Scene Management Large Crowds][7]

### FrameGraph

 * [FrameGraph: Extensible Rendering Architecture in Frostbite][35]

### Stingray Renderer Walkthrough

 * bitsquid, 被 autodesk 买了之后，改名为 stingray
 * blog: [http://bitsquid.blogspot.com/][46]
 * 两位核心开发者：Niklas & Tobias
 * 离开 autodesk 后，他们最新搞的引擎：[Our Machinery][37]

Stringray Engine Code Walkthrough

 * Youtube Video by Niklas
 * [Stingray Engine Code Walkthrough][36]

Stringray Renderer Walkthrough

 * Blog by Tobias
 * [Overview][38]
 * [Resources & Resource Contexts][39]
 * [Render Contexts][40]
 * [Sorting][41]
 * [RenderDevice][42]
 * [RenderInterface][43]
 * [Data-driven rendering][44]
 * [stingray-renderer & mini-renderer][45]


## Particle System

 * [2018 - GDC - Frostbite GPU Emitter Graph System][29]
 * [2018 - GDC - Beyond Emitters: Shader and Surface Driven GPU Particle FX Techniques][34]
 * [2014 - GDC - the inFAMOUS: Second Son Particle System Architecture][28]
 * [2014 - GDC - Compute-Based GPU Particle Systems][27]
 * [2014 - AMD - GPU Particles Sample based on DirectX11][30]


## Textures

### ptex (Per-Face Texture Mapping)

 * 电影行业用的技术，没看到现有引擎用于游戏
 * [http://ptex.us/][23]
 * [2011 - SIGGRAPH - Per-Face Texture Mapping for Realtime Rendering][24]
 * [2013 - NVIDIA - Eliminating Texture Waste: Borderless Ptex][25]
 * [Ptex and PRT Technology Preview][26]


## HDR - High Dynamic Range Rendering

 * [GDC2010 - Uncharted 2: HDR Lighting][10]
 * [GDC2005 - Far Cry and DirectX9][11]
 * GDC2003 - Frame Buffer Postprocessing Effects in DOUBLE-S.T.E.A.L（ppt、[voice][12]）


## GI - Global Illumination

#### SSAO - Screen Space Ambient Occlusion

 * 2011 - GEMS8 - Principles and Practice of Screen Space Ambient Occlusion (Starcraft II)
 * 2009 - ShaderX7 - Variance Methods for Screen-Space Ambient Occlusion
 * 2009 - ShaderX7 - Screen-Space Ambient Occlusion (Crysis)
 * [LearnOpenGL - SSAO][14]

#### SSDO - Screen Space Directional Occlusion

 * 2009 - Approximating Dynamic Global Illumination in Image Space

#### HBAO - Horizon-Based Ambient Occlusion

 * [2009 - SIGGRAPH - Multi-Layer Dual-Resolution Screen-Space Ambient Occlusion][13]
 * [2008 - SIGGRAPH - Image-Space Horizon-Based Ambient Occlusion][16]
 * [2008 - NVidia - Screen Space Ambient Occlusion][17]
 * [NVIDIA ShadowWorks][15]，有 HBAO+ 的实现

#### LPV & SVOGI & VXGI

 * [About LPV, SVOGI and VXGI][18]
 * 
 * LPV - 作者 Anton Kaplanyan
 * [2009 - SIGGRAPH - Light Propagation Volumes in CryENGINE 3][19]
 * 
 * SVOGI - 作者 Andrew Scheidecker，[https://www.scheidecker.net/][20]
 * 在 UE4 Elemental Tech Demo 中实现了 SVOGI
 * 但因为 XBOXONE & PS4 的性能不足，最终从引擎中删除了这部分的实现
 * 目前 CRYENGINE 5 中实现了 SVOGI
 * 
 * VXGI - Nvidia 设计的，和 SVOGI 同类的技术
 * [Voxel Cone Tracing and Sparse Voxel Octree for Real-time Global Illumination][21]
 * [https://developer.nvidia.com/vxgi][22]


## Reference

 * [2018 - GDC Presentations][31]
 * [2017 - GDC Presentations][32]
 * [2016 - GDC Presentations][33]


[1]:https://takahiroharada.files.wordpress.com/2015/04/forward_plus.pdf
[2]:https://github.com/WindyDarian/Vulkan-Forward-Plus-Renderer
[3]:https://github.com/GPUOpen-LibrariesAndSDKs/ForwardPlus11/
[4]:https://www.cnblogs.com/gongminmin/archive/2012/04/22/2464982.html
[5]:https://gpuopen.com/learn/gcn-shader-extensions-for-direct3d-and-vulkan/
[6]:https://www.advances.realtimerendering.com/s2015/aaltonenhaar_siggraph2015_combined_final_footer_220dpi.pdf
[7]:https://drivers.amd.com/misc/siggraph_asia_08/GPUBasedSceneManagementLargeCrowds.pdf
[8]:https://twvideo01.ubm-us.net/o1/vault/gdc2019/presentations/Drazhevskyi_Oleksandr_GPU_Driven_Rendering.pdf
[9]:https://www.gdcvault.com/play/1023109/Optimizing-the-Graphics-Pipeline-With
[10]:https://www.gdcvault.com/play/1012351/Uncharted-2-HDR
[11]:https://ia800902.us.archive.org/25/items/crytek_presentations/GDC2005_FarCryAndDX9.ppt
[12]:https://www.gdcvault.com/play/1022664/Frame-Buffer-Postprocessing-Effects-in
[13]:https://developer.download.nvidia.cn/presentations/2009/SIGGRAPH/Bavoil_MultiLayerDualResolutionSSAO.pdf
[14]:https://learnopengl.com/Advanced-Lighting/SSAO
[15]:https://developer.nvidia.com/shadowworks
[16]:https://developer.download.nvidia.com/presentations/2008/SIGGRAPH/HBAO_SIG08b.pdf
[17]:https://developer.download.nvidia.cn/SDK/10.5/direct3d/Source/ScreenSpaceAO/doc/ScreenSpaceAO.pdf
[18]:https://www.zhihu.com/question/28295455
[19]:http://advances.realtimerendering.com/s2009/
[20]:https://www.scheidecker.net/
[21]:https://on-demand.gputechconf.com/gtc/2012/presentations/SB134-Voxel-Cone-Tracing-Octree-Real-Time-Illumination.pdf
[22]:https://developer.nvidia.com/vxgi
[23]:http://ptex.us/
[24]:https://developer.download.nvidia.cn/assets/gamedev/docs/RealtimePtex-siggraph2011.pdf
[25]:https://developer.nvidia.com/sites/default/files/akamai/gamedev/docs/Borderless%20Ptex.pdf
[26]:https://gpuopen.com/archived/radeon-hd-7900-series-graphics-real-time-demos/
[27]:http://twvideo01.ubm-us.net/o1/vault/GDC2014/Presentations/Gareth_Thomas_Compute-based_GPU_Particle.pdf
[28]:https://www.suckerpunch.com/iss-particles-gdc2014/
[29]:https://www.ea.com/frostbite/news/frostbite-gpu-emitter-graph-system
[30]:https://github.com/GPUOpen-LibrariesAndSDKs/GPUParticles11
[31]:https://knarkowicz.wordpress.com/2018/03/22/gdc-2018-presentations/
[32]:https://knarkowicz.wordpress.com/2017/03/01/gdc-2017-presentations/
[33]:https://knarkowicz.wordpress.com/2016/03/21/gdc-2016-presentations/
[34]:https://www.dropbox.com/s/guyvljewxhjssyr/ccoffin_GDC18_Version030_final.pptx?dl=0
[35]:https://www.gdcvault.com/play/1024612/FrameGraph-Extensible-Rendering-Architecture-in
[36]:https://www.youtube.com/playlist?list=PLUxuJBZBzEdxzVpoBQY9agA8JUgNkeYSV
[37]:https://ourmachinery.com/post/
[38]:http://bitsquid.blogspot.com/2017/02/stingray-renderer-walkthrough-1-overview.html
[39]:http://bitsquid.blogspot.com/2017/02/stingray-renderer-walkthrough-2.html
[40]:http://bitsquid.blogspot.com/2017/02/stingray-renderer-walkthrough-3-render.html
[41]:http://bitsquid.blogspot.com/2017/02/stingray-renderer-walkthrough-4-sorting.html
[42]:http://bitsquid.blogspot.com/2017/02/stingray-renderer-walkthrough-5.html
[43]:http://bitsquid.blogspot.com/2017/02/stingray-renderer-walkthrough-6.html
[44]:http://bitsquid.blogspot.com/2017/03/stingray-renderer-walkthrough-7-data.html
[45]:http://bitsquid.blogspot.com/2017/03/stingray-renderer-walkthrough-8.html
[46]:http://bitsquid.blogspot.com/
