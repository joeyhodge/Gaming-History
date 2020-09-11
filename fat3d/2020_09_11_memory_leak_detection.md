# Memory Leak Detection


## Using MSVC CRT DbgMalloc

 * 参考 ["Detecting Memory Leaks"][1]
 * 缺点：只有 Debug Build 才能启用

```C++
#if defined(FAT_ENABLE_MEMORY_LEAK_DETECTION)
#  define _CRTDBG_MAP_ALLOC
#  include <stdlib.h>
#  include <crtdbg.h>
#endif

#if defined(FAT_ENABLE_MEMORY_LEAK_DETECTION)
#define _FatMalloc(size, align) _aligned_malloc_dbg(size, align, __FILE__, __LINE__)
#define FatMalloc(size) _FatMalloc(size, 16)

#define FatFree(p) _aligned_free_dbg(p)

#define FatNew(Type, ...) new(_NORMAL_BLOCK, __FILE__, __LINE__) Type(__VA_ARGS__)
#define FatNewArray(Type, Count) new(_NORMAL_BLOCK, __FILE__, __LINE__) Type[Count]
#define FatDelete(Pointer) delete Pointer
#define FatDeleteArray(Pointer) delete [] Pointer
#endif

void InitMemoryLeakCheck()
{
#if defined(FAT_ENABLE_MEMORY_LEAK_DETECTION)
    FatLog(L"<MemCheck>: Init");

    // Perform automatic leak checking at program exit
    _CrtSetDbgFlag(_CrtSetDbgFlag(_CRTDBG_REPORT_FLAG) | _CRTDBG_ALLOC_MEM_DF | _CRTDBG_LEAK_CHECK_DF);
#endif
}
```

用法

```C++
int main()
{
    void *p = FatMalloc(10);
    return 0;
}
```

结果

```
Detected memory leaks!
Dumping objects ->
...\main.cpp(42) : {343} normal block at 0x000001C89F30A2D0, 41 bytes long.
 Data: <  0             > D0 A2 30 9F C8 01 00 00 ED ED ED ED ED ED ED ED 
Object dump complete.
```

[1]:https://www.flipcode.com/archives/Detecting_Memory_Leaks.shtml
