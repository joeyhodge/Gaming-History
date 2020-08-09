# CryENGINE Technical Notes


## CryENGINE History

 * [CryEngine Video Game Engine Review][1]，一些 CryENGINE 做的游戏
 * 手游时代来领，CRYENGINE 5 开源，[https://github.com/CRYTEK/CRYENGINE][2]
 * 2013年11月，Ryse: Son of Rome
 * 2013年2月，Crysis 3
 * 2011年，CryENGINE 3 和 Crysis 2
 * 2007年，CryENGINE 2 和 Crysis
 * 2004年，CryENGINE 1 和 Far Cry。版权纠纷，FarCry 后来给了 Ubisoft

### Crysis 2 截图

![](images/2020_08_08_cryengine_technical_notes/crysis2.png)


## Presentations & Papers

### 2013

 * [FMX - The Art and Technology behind Crysis 3][14]
 * GDC - How CryENGINE 3 and AMD GCN Architecture Gave Birth to a Red Gem - Project Phoenix
 * [GDC - Moving to the Next Generation - The Rendering Technology of Ryse][15]
 * [GDC - Advanced Visual Effects with DirectX 11: The Rendering Technologies of Crysis 3][16], [GDC Video][17]
 * [GDC Europe - Shining the Light on Crysis 3][18]
 * [SIGGRAPH - CryENGINE 3 Graphic Gems][19]
 * SIGGRAPH - Defining the Next Next-Gen
 * [SIGGRAPH - Playing with Real-Time Shadows][20]

### 2012

 * SIGGRAPH - VFX for Games - Destruction Setpieces
 * SIGGRAPH - VFX for Games - Particle Effects

### 2011

 * [GAMEFEST - CryENGINE 3 Rendering Techniques][10]
 * [GDC Europe - Lighting in Crysis 2][11]
 * [SIGGRAPH - Anti-Aliasing Methods in CryENGINE 3][12]
 * [SIGGRAPH - Secrets of CryENGINE 3 Graphics Technology][13]
 * SIGGRAPH - Spherical Skinning with Dual-Quaternions and QTangents

### 2010

 * GDC China - The Future of Game Engines - Towards Real-time Photo-realistic Rendering and Natural Character Animation
 * GDC Europe - AAA Stereo-3D in CryENGINE 3
 * GDC Europe - Bringing Stereo to Consoles
 * HPGC - Future Graphics in Games
 * [I3D - Cascaded Light Propagation Volumes for Real Time Indirect Illumination][8]
 * SIGGRAPH - Real-time Diffuse Global Illumination in CryENGINE 3
 * [SIGGRAPH - CryENGINE 3: Reaching the Speed of Light][9]

### 2009

 * [SIGGRAPH - Light Propagation Volumes in CryENGINE 3][5]
 * [TGC - A Bit More Deferred CryENGINE 3][6]

### 2007

 * [SIGGRAPH - Finding Next Gen - CryENGINE 2][3]

### Documentation

 * [https://docs.cryengine.com/][4]
 * [all Crytek free-talks on GDC][7]


## Core Graphics Developers

### Martin Mittring

 * [SIGGRAPH2007 - Finding Next Gen - CryENGINE 2][3]
 * [TGC2009 - A Bit More Deferred CryENGINE 3][6]
 * "Unreal Engine 4 Elemental demo" - SIGGRAPH 2012，说明已经去 Epic 了
 * https://docs.unrealengine.com/udk/Three/ContentBlog.html，可以看到 mittring 很多在 unreal 的成果

### Tiago Sousa

 * Mittring 离开之后，Sousa 接班。2014 年离开 Crytek，去 idsoftware 接班 John Carmack
 * [SIGGRAPH2011 - Secrets of CryENGINE 3 Graphics Technology][13]
 * [SIGGRAPH2013 - CryENGINE 3 Graphic Gems][19]

### Timur Davidenko

 * 代码中有很多 Timur 写的内容
 * 没有 paper，写了很多引擎代码
 * Technical Director

### Nicolas Schulz

 * 主要负责游戏的渲染，比如 Ryse
 * GDC2014 - Moving to the Next Generation - The Rendering Technology of Ryse

### Anton Kaplanyan

 * 研究员，an ex-R&D at Crytek，实现了 CryENGINE 3 LPV
 * [个人网页][22]
 * [SIGGRAPH2009 - Light Propagation Volumes in CryENGINE 3][5]
 * SIGGRAPH2010 - Real-time Diffuse Global Illumination in CryENGINE 3
 * [SIGGRAPH2010 - CryENGINE 3: Reaching the Speed of Light][9]
 * [LPV实现解读][21]


## Tools

### QuickBMS

 * http://aluigi.altervista.org/quickbms.htm
 * QuickBMS + Crysis 2 script
 * quickbms.exe crysis2.bms Textures.pak OutputFolder


[1]:https://www.gamedesigning.org/engines/cryengine/
[2]:https://github.com/CRYTEK/CRYENGINE
[3]:https://developer.amd.com/wordpress/media/2013/02/Chapter8-Mittring-Finding_NextGen_CryEngine2.pdf
[4]:https://docs.cryengine.com/
[5]:http://advances.realtimerendering.com/s2009/
[6]:https://www.slideserve.com/yama/a-bit-more-deferred-cryengine-3
[7]:https://www.gdcvault.com/search.php#&conference_id=&category=free&firstfocus=&keyword=Crytek
[8]:https://www.realtimerendering.com/blog/cascaded-light-propagation-volumes-for-indirect-illumination/
[9]:http://advances.realtimerendering.com/s2010/
[10]:https://www.slideshare.net/TiagoAlexSousa/cryengine-3-rendering-techniques
[11]:https://www.gdcvault.com/play/1014915/Lighting-in-Crysis
[12]:https://www.slideshare.net/TiagoAlexSousa/antialiasing-methods-in-cryengine-3
[13]:http://advances.realtimerendering.com/s2011/
[14]:https://www.slideshare.net/TiagoAlexSousa/the-art-and-technology-behind-crysis-3-fmx-2013
[15]:https://gdcvault.com/play/1020432/Moving-to-the-Next-Generation
[16]:https://www.slideshare.net/TiagoAlexSousa/rendering-technologies-from-crysis-3-gdc-2013
[17]:https://gdcvault.com/play/1017626/Advanced-Visual-Effects-with-DirectX
[18]:https://www.gdcvault.com/play/1019235/Shining-the-Light-on-Crysis
[19]:http://advances.realtimerendering.com/s2013/
[20]:https://www.realtimeshadows.com/sites/default/files/Playing%20with%20Real-Time%20Shadows_0.pdf
[21]:https://ericpolman.com/2016/06/28/light-propagation-volumes/
[22]:http://kaplanyan.com/
