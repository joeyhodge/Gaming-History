# FFmpeg 小试牛刀

最近项目中需要解码 H.264 的视频，小试了一下 ffmpeg，果然强大。

 * 官网，[http://ffmpeg.org/][1]
 * wiki，[https://trac.ffmpeg.org/wiki][2]
 * API，[http://ffmpeg.org/doxygen/trunk/][3]


## Compile FFmpeg from source

4.x 开始，不支持 VS2012，最后选用了 3.x 版本的 source。

 * 参考资料，[https://trac.ffmpeg.org/wiki/CompilationGuide/MSVC][5]
 * 构建了一个编译仓库，[https://github.com/kasicass/ffmpeg-forfun][6]


## 小谈视频编码/解码

.avi, .mp4 这些文件，都是一些壳子，包含了一些基本信息。而 mpeg4, h.264 等等则是具体内容的编码/解码算法。

 * .avi / mp4 的读取(muxer)和写入(demuxer)，由 libavformat 负责
 * mpeg4 / h.264 等的读取(decoder)和写入(encoder)，由 libavcodec 负责

完成的视频播放例子，可以参见

 * [https://github.com/kasicass/ffmpeg-forfun][6]


## 自定义I/O

如果想自定义 I/O 的行为，需要去实现 avio 接口。

 * [http://ffmpeg.org/doxygen/trunk/avio_reading_8c-example.html][7]

```C
/* note: the internal buffer could have changed, and be != avio_ctx_buffer */
// 这个要注意，很容易就 free 到 avio_ctx_buffer。
if (avio_ctx)
  av_freep(&avio_ctx->buffer);
avio_context_free(&avio_ctx);
```


## 参考资料

 * 《[FFmpeg从入门到精通][4]》，代码维护者的作品，好书、好书。

[1]:http://ffmpeg.org/
[2]:https://trac.ffmpeg.org/wiki
[3]:http://ffmpeg.org/doxygen/trunk/
[4]:https://item.jd.com/12349436.html
[5]:https://trac.ffmpeg.org/wiki/CompilationGuide/MSVC
[6]:https://github.com/kasicass/ffmpeg-forfun
[7]:http://ffmpeg.org/doxygen/trunk/avio_reading_8c-example.html
