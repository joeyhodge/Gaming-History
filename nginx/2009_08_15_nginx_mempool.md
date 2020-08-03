# [nginx] ngx mempool

基于 nginx 0.7.61

俄罗斯同学的 http server，据说很快很好用。我向来喜欢短小精悍的家伙，虽然我自己不做web，不过还是可以研究下，看看web的应用环境到底啥情况。

 * [http://nginx.net/][1]
 * [http://wiki.nginx.org/][2]

nginx 的代码结构很简洁，分为下面几个目录。

 * [core], [event], [os]，此三者就是核心部分了，其中 event 管理事件分发，而 os 则是系统相关内容的封装，比如thread。
 * [http], [mail], 专项处理 http/mail 两大类协议。
 * [misc]，杂项，每个项目里总会有点不知道归属哪个类别的东西。

看代码，自己总喜欢先翻翻 util func 之类的辅助类，一般对其他模块依赖比较少，读起来比较畅快。

nginx 实现了简单的内存池管理。(core/ngx_palloc.c|.h)

  1. 小块内存，申请了不释放 （free 整个 pool 的时候才一并干掉）
  2. 大内存，直接找系统 malloc/free

小内存块不释放的原因，个人理解为每次 http request 的过程总不会太长，一次 http request 后，所有的 pool 都会被干掉。多次分配一次释放，倒是清爽迅捷。而且统一从 pool 分配，很容易跟踪内存分配的情况。同时方便了代码的撰写，比如：

```C
create_xxx(ngx_pool_t *pool)
{
    a = ngx_palloc(pool, size_a);
    if ( a == NULL ) { return ERR_A; }
    ...
    b = ngx_palloc(poo, size_b);
    if ( b == NULL ) { return ERR_B; }
    return CREATE_OK;
}
```

外层函数根据返回值判断，如果失败，可以整体删除 pool 即可。

ngx mempool 里稍微有点 trick 的地方，就是 struct ngx_pool_s 在 ngx_create_pool/ngx_palloc_block 两个地方，采用了不同的用法。其中 palloc_block 的 pool_s 并不会使用 ngx_pool_data_t d 之外的任何 field。

```C
struct ngx_pool_s {
    ngx_pool_data_t       d;
    size_t                max;
    ngx_pool_t           *current;
    ngx_chain_t          *chain;
    ngx_pool_large_t     *large;
    ngx_pool_cleanup_t   *cleanup;
    ngx_log_t            *log;
};
```

[1]:http://nginx.net/
[2]:http://wiki.nginx.org/
