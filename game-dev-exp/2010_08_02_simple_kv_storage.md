# [game-dev] simple key/value storage

最近根据项目的需求，设计了一个简单的 key/value storage 模块。

## 基本需求

MainThread 需要 read, modify 数据，同时希望所有 modified data，可以在某一个时刻同时保存到磁盘，保证数据一致性。

MainThread 负责从磁盘 block-read 数据，modify 在内存中完成，然后定期存盘放到另一个 thread 中。

## 实现

使用者通过 "task/2010/spring" 这样的 key，来获取一个节点数据，然后可以随意 modify 此数据，最后只要 commit 一下，则会立即序列化此节点，同时放入 dirty_list 中，再定期写到磁盘中。

至于每个节点如何序列化，则允许使用者自己注册 hook func 来控制。

treedb 头文件：

```C
/* treedb.h */
#include <stdbool.h>

struct TreeDB;
struct TreeDBIter;

typedef void* (*tdb_hook_new) (void);
typedef void  (*tdb_hook_delete) (void *data);
typedef char* (*tdb_hook_serialize) (void *data, unsigned int *buflen);
typedef void* (*tdb_hook_unserialize) (char *buf, unsigned int buflen);

struct TreeDBHook
{
    tdb_hook_new         new;
    tdb_hook_delete      delete;

    tdb_hook_serialize   serialize;
    tdb_hook_unserialize unserialize;
};


struct TreeDB *tdb_open(const char *dbpath, struct TreeDBHook* hook);
void tdb_sync(struct TreeDB *tdb);
void tdb_close(struct TreeDB *tdb);

void *tdb_leafnode_get_data(struct TreeDB *tdb, const char *key);
void tdb_leafnode_delete(struct TreeDB *tdb, const char *key);
void tdb_leafnode_commit(struct TreeDB *tdb, const char *key);

struct TreeDBIter *tdb_midnode_new_iter(struct TreeDB *tdb, const char *key);
void tdb_midnode_del_iter(struct TreeDBIter *iter);
bool tdb_midnode_iter_has_next(struct TreeDBIter *iter);
void *tdb_midnode_iter_next(struct TreeDBIter *iter, const char **key); // only iterater leafnodes

void tdb_midnode_delete(struct TreeDB *tdb, const char *key);

void tdb_walk(struct TreeDB *tdb);      // for debug
```

使用代码：

```C
#include "tdb.h"
#include <stdio.h>
#include <stdlib.h>

struct MyData
{
    int a;
    float b;
};

void* mydata_new(void)
{
    struct MyData *d = (struct MyData *) malloc(sizeof(struct MyData));
    d->a = 10;
    d->b = 2.3;
    return d;
}

void mydata_delete(void *data)
{
    free(data);
}

char* mydata_serialize(void *data, unsigned int *buflen)
{
    struct MyData *d = (struct MyData *) data;
    char *buf = malloc(80);
    sprintf(buf, "%d %f", d->a, d->b);
    *buflen = 80;
    return buf;
}

void* mydata_unserialize(char *buf, unsigned int buflen)
{
    struct MyData *d = (struct MyData *) malloc(sizeof(struct MyData));
    sscanf(buf, "%d %f", &(d->a), &(d->b));
    return d;
}

int main()
{
    struct TreeDBHook hook = { mydata_new, mydata_delete, mydata_serialize, mydata_unserialize };
    struct TreeDB *tdb;
    struct MyData *d;
    struct TreeDBIter *iter;

    tdb = tdb_open("./tmpdb", &hook);

    // new leafnode
    d = (struct MyData *) tdb_leafnode_get_data(tdb, "task/2010/spring");
    d->a = 1001;
    tdb_leafnode_commit(tdb, "task/2010/spring");

    d = (struct MyData *) tdb_leafnode_get_data(tdb, "task/2010/summon");
    d->a = 1002;
    tdb_leafnode_commit(tdb, "task/2010/summon");

    d = (struct MyData *) tdb_leafnode_get_data(tdb, "task/2010/autumn");
    d->a = 1003;
    tdb_leafnode_commit(tdb, "task/2010/autumn");

    d = (struct MyData *) tdb_leafnode_get_data(tdb, "task/2011/winter");
    d->a = 1004;
    tdb_leafnode_commit(tdb, "task/2011/winter");

    tdb_walk(tdb);

    // iterate one midnode's leafnodes
    printf("========== iter ==========\n");
    iter = tdb_midnode_new_iter(tdb, "task/2010");
    if ( iter )
    {
        while ( tdb_midnode_iter_has_next(iter) )
        {
            const char *key;
            d = (struct MyData *) tdb_midnode_iter_next(iter, &key);
            printf("%s: %d, %f\n", key, d->a, d->b);
        }
    }
    tdb_midnode_del_iter(iter);
    printf("========== end iter ==========\n");

    // delete leafnode
    tdb_leafnode_delete(tdb, "task/2010/spring");
    tdb_walk(tdb);

    // delete midnode
    tdb_midnode_delete(tdb, "task/2010");
    tdb_walk(tdb);

    tdb_sync(tdb);
    tdb_close(tdb);
    return 0;
}
```

运行结果：

```C
$ ./a.out 
=========== walk ============
[task] = 0x800a09060
        [2010] = 0x800a090c0
                <autumn>
                <spring>
                <summon>
        [2011] = 0x800a09220
                <winter>
=============================
========== iter ==========
autumn: 1003, 2.300000
spring: 1001, 2.300000
summon: 1002, 2.300000
========== end iter ==========
=========== walk ============
[task] = 0x800a09060
        [2010] = 0x800a090c0
                <autumn>
                <summon>
        [2011] = 0x800a09220
                <winter>
=============================
=========== walk ============
[task] = 0x800a09060
        [2011] = 0x800a09220
                <winter>
=============================
```

最后 tmpdb 目录结构如下：

```
[tmpdb]
  + [task]
       +[2011]
           + <winter>
```

每个 leafnode，相当于一个 file。每个 midnode 相当于一个 directory。
