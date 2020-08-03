# 回到传统：关于glTexSubImage2D的讨论汇总

fatfatson 的一篇文章，转发下。

我曾经在之前的一篇日志里提到"动态合批"这个概念，这是当时为解决在dx9下DP数量过多引起性能急剧下降而采取的一种非主流做法。事实上这种做法收到非常好的效果，在一个intel G41集成显卡上，大约300个DP就可以把CPU撑到极限，而一旦合成一批后，CPU占用戏剧般的下降到一般水平，整个程序变得异常流畅。在更好的一点的显卡上，DP的影响不再那么明显，但此种优化的效果仍是值得尝试的。然而，DP合并的关键在于纹理的合并，而这恰恰是一个难以捉摸的问题。要动态、频繁地将一些小纹理，以局部更新的方式上传到显存中的一张大纹理上，似乎并不是传统3D应用的主要需求，因而各家的显卡、驱动甚至图形API对此的支持相差甚巨，有的具有完美表现，有的则几乎不能工作。由于这个问题已经严重影响到我对新引擎渲染流程的设计决定，我不得不做了大量测试，及在网上搜索了大量相关资料探讨，现将其统录下于，以兹备忘。

## 本人的研究测试

一、在dx9下，update texture有两种方式。一是对texture lock/unlock，在返回的指针上直接填写数据；二是用device.UpdateSurface这个api。前者的好处是源数据可以以任何形式保存，直到填写的时候才解成texture相应格式即可，这可以极大减少内存占用，但是lock这个操作在N卡的速度很慢，在GT220/310这种并不低端的卡上，对一张2048x2048的texture做lock/unlock，竟然需要10ms；后者则要求数据源必须创建成dx texture的形式，这使动态解压变得不可能，但其好处是在N卡、A卡上速度都较快，几乎可以认为是0开销，我猜测它应该是用DMA去做上传。lock的开销曾在几张不同显卡上做了测试，数据将附在文尾。

二、在opengl下，只有一种方法即glTexSubImage2D。但它的数据源有两种形式，一是裸数据形式，二是PBO形式，但PBO要3.0才有，且在OpenGL ES里是没有的。不管是裸数据还是PBO，在桌面版GL下也都是很快的，几乎是不到1个ms。但是在移动平台的GL ES下，速度下降得非常惊人。在flash11、google native client、android都做过测试，其中flash11直接支持局部更新，整图更新要7个ms，native client大概要2个ms，小米android上则令人吃惊的达到平均3~4ms，最高竟达5000ms（不规则的间断出现此峰值）。

从以上结果来看，在OpenGL ES上要想频繁且高效的利用glTexSubImage2D几乎是不可能的了，而严重依赖于此的动态合批机制也就很成问题。要做一个跨平台的3D画2D引擎也就变得两难：如果坚持合批，那么在移动设备上效率很低；如果不合批，那么在Windows/dx9上效率很低。最终决定，还要再仔细思考比较。但是先把最近搜索到的资料整理一下：

## 网上搜到的资料

一、[最核心的一篇][1]，其中提到面临与我同样的困难。但是它通过android内部的一个未公开的api即GraphicBuffer，结合EGL扩展eglCreateImageKH来实现高速、异步纹理更新。原理大致是将texture与一内核缓冲区绑定，对此缓冲区的更改就可直接反映到纹理上。方法虽好，但问题不小，一是既然是未公开api，那就面临着系统、设备更新等不兼容问题，作者自己也提到并不是所有的android设备都支持此做法，二是除了android，还有其它的移动系统，没有此扩展怎么办。
　　
二、GraphicBuffer 的代码在[这里][2]。
　　
三、此文讲的另一个问题：[glReadPixels慢][3]。这个和glTexSubImage2D是一对相反但同理的问题，前者是要拖下来，后者是要传上去，其中一个解决了另一个自然也解决了。它的做法与第一篇一样，甚至还详细列出了各个步骤和具体代码。
　　
四、[同样的问题在iphone上的讨论][4]。

五、[也是被这个问题困扰，没有解决][5]。

六、[也是被这个问题困扰][6]，回复说最好别用 glTexSubImage2D-_-#

七、[也是被这个问题困扰][7]。回复提到了一个叫GL_TEXTURE_BUFFER的东东，但这个东西显然OpenGL ES里是没有的

八、[这人应该是把texture当画布用][8]，手动把所有东西在texture上"画"好，再"真正画"到framebuffer上。当他发现用glTexSubImage2D来"画"很慢时，就改用别的方式了。可惜我跟他的目的不一样，不能像他那样换掉。

九、[这人用glTexSubImage2D来动态上传光照图][9]。算是跟我的做法有点像吧。可惜也没解决。有意思的是他说网上找的各种方案都试过了都是没用的。

十、[这人说glTexSubImage2D慢是因为每次要重新生成mipmap的原因][10]。可惜我根本就没有用到mipmap，一开始创建的时候就指定为1了。

十一、[一样的问题][11]。回复里有提到一点新颖的：每次更新纹理哪怕是局部的，也要导致整张图重新"格式化"成"硬件认识的格式"。

十二、[提到update导致pipeline stall的问题][12]。但我现在的用法是，先做完所有的局部更新，再画，然后再下一轮更新、画，并未穿插调用，应该不存他提到问题。况且这个做法在桌面GL上表现也是良好的，应该还是GL ES不能很好支持？


## 附：tex lock测试数据

### intel g41

```
fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:128
 cost: wd:45/4 ws:1/0 ps:4/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:256
 cost: wd:1/0 ws:1/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:512
 cost: wd:1/0 ws:0/0 ps:1/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:1024
 cost: wd:3/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:2048
 cost: wd:6/0 ws:1/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:4096
 cost: wd:20/2 ws:1/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:8192
CreateTexture failed

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:128
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:256
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:512
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:1024
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:2048
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:4096
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:8192
CreateTexture failed

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:128
 cost: wd:105/10 ws:6/0 ps:5/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:256
 cost: wd:4/0 ws:16/1 ps:16/1

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:512
 cost: wd:11/1 ws:61/6 ps:64/6

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:1024
 cost: wd:36/3 ws:248/24 ps:238/23

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:2048
 cost: wd:211/21 ws:1093/109 ps:1078/107

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:4096
 cost: wd:837/83 ws:4313/431 ps:4287/428

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:8192
CreateTexture failed

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:128
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:256
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:512
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:1024
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:2048
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:4096
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:8192
CreateTexture failed
```

### gt430

```
fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:128
 cost: wd:74/7 ws:5/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:256
 cost: wd:146/14 ws:11/1 ps:1/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:512
 cost: wd:78/7 ws:46/4 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:1024
 cost: wd:40/4 ws:179/17 ps:1/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:2048
 cost: wd:223/22 ws:750/75 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:4096
 cost: wd:534/53 ws:2915/291 ps:19/1

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:8192
 cost: wd:5591/559 ws:13364/1336 ps:1/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:128
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:256
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:512
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:1024
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:2048
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:4096
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:8192
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:128
 cost: wd:29/2 ws:14/1 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:256
 cost: wd:74/7 ws:4/0 ps:1/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:512
 cost: wd:170/17 ws:11/1 ps:1/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:1024
 cost: wd:93/9 ws:62/6 ps:1/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:2048
 cost: wd:70/7 ws:214/21 ps:1/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:4096
 cost: wd:148/14 ws:783/78 ps:61/6

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:8192
 cost: wd:1041/104 ws:2950/295 ps:1/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:128
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:256
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:512
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:1024
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:2048
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:4096
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:8192
 cost: wd:0/0 ws:0/0 ps:0/0
```

### x1300pro

```
fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:128
 cost: wd:28/0 ws:15/0 ps:9/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:256
 cost: wd:13/0 ws:5/0 ps:6/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:512
 cost: wd:20/0 ws:10/0 ps:5/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:1024
 cost: wd:13/0 ws:5/0 ps:6/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:2048
 cost: wd:16/0 ws:5/0 ps:6/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:4096
 cost: wd:28/0 ws:6/0 ps:5/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:8192
CreateTexture failed

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:128
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:256
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:512
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:1024
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:2048
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:4096
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:8192
CreateTexture failed

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:128
 cost: wd:13/0 ws:5/0 ps:6/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:256
 cost: wd:13/0 ws:5/0 ps:6/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:512
 cost: wd:12/0 ws:6/0 ps:5/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:1024
 cost: wd:12/0 ws:19/0 ps:7/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:2048
 cost: wd:13/0 ws:5/0 ps:6/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:4096
 cost: wd:18/0 ws:5/0 ps:6/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:8192
CreateTexture failed

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:128
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:256
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:512
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:1024
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:2048
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:4096
 cost: wd:1/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:8192
CreateTexture failed
```

### 7600GT

```
fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:128
 cost: wd:4/0 ws:131/13 ps:2/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:256
 cost: wd:4/0 ws:85/8 ps:12/1

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:512
 cost: wd:5/0 ws:241/24 ps:43/4

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:1024
 cost: wd:4/0 ws:1087/108 ps:288/28

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:2048
 cost: wd:872/87 ws:2850/285 ps:1246/124

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:4096
 cost: wd:24511/2451 ws:13012/1301 ps:7869/786

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_DEFAULT, size:8192
CreateTexture failed

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:128
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:256
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:512
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:1024
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:2048
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:4096
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_A8R8G8B8, pool:D3DPOOL_SYSTEMMEM, size:8192
CreateTexture failed

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:128
 cost: wd:1/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:256
 cost: wd:0/0 ws:0/0 ps:1/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:512
 cost: wd:0/0 ws:1/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:1024
 cost: wd:0/0 ws:0/0 ps:1/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:2048
 cost: wd:1/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:4096
 cost: wd:0/0 ws:0/0 ps:1/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_DEFAULT, size:8192
CreateTexture failed

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:128
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:256
 cost: wd:0/0 ws:1/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:512
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:1024
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:2048
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:4096
 cost: wd:0/0 ws:0/0 ps:0/0

fmt:D3DFMT_DXT5, pool:D3DPOOL_SYSTEMMEM, size:8192
CreateTexture failed
```

[1]:http://snorp.net/2011/12/16/android-direct-texture.html
[2]:http://source-android.frandroid.com/frameworks/base/include/ui/GraphicBuffer.h:
[3]:http://forums.arm.com/index.php?/topic/15782-glreadpixels/
[4]:http://www.cocoachina.com/bbs/simple/?t5930.html
[5]:http://stackoverflow.com/questions/5281598/android-opengl-es-gltexsubimage2d-hiccup-the-first-time-called-after-the-fir
[6]:http://www.imgtec.com/forum/forum_posts.asp?TID=115&title=texture-update-using-gltexsubimage2d
[7]:http://stackoverflow.com/questions/7808146/gltexsubimage2d-extremely-slow-on-intel-video-card
[8]:https://groups.google.com/forum/?fromgroups=#!topic/android-ndk/-uTRqhRzEfI
[9]:http://mhquake.blogspot.com/2010/07/gltexsubimage2d-and-quest-for-fast.html
[10]:http://www.clearchain.com/blog/posts/opengl-gltexsubimage2d-very-slow-a-solution
[11]:http://www.khronos.org/message_boards/viewtopic.php?f=4&t=1274
[12]:http://www.gamedev.net/topic/588328-gltexsubimage2d-performance/
