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


## My Memory Leak Detection Method

 * 自己实现一套
 * 维护一个链表，记录所有分配的内存块
 * malloc/realloc/operator new, 新增记录
 * free/operator delete, 删除记录
 * 为啥会有 "void operator delete(void* p, const wchar_t* file, int line)"，参考[这里][2]
 * [代码实现][3]

```C++
#define FatMalloc(size)          Fat::Memory::MallocDbg(size, FAT_CONCAT(L,__FILE__), __LINE__)
#define FatRealloc(p, size)      Fat::Memory::ReallocDbg(p, size, FAT_CONCAT(L,__FILE__), __LINE__)
#define FatFree(p)               Fat::Memory::FreeDbg(p)

#define FatNew(Type, ...)        new(FAT_CONCAT(L,__FILE__), __LINE__) Type(__VA_ARGS__)
#define FatNewArray(Type, Count) new(FAT_CONCAT(L,__FILE__), __LINE__) Type[Count]
#define FatDelete(Pointer)       delete Pointer
#define FatDeleteArray(Pointer)  delete [] Pointer

void operator delete(void* p, const wchar_t* file, int line) noexcept;
void operator delete[](void* p, const wchar_t* file, int line) noexcept;

void* operator new(size_t n, const wchar_t* file, int line) noexcept(false);
void* operator new[](size_t n, const wchar_t* file, int line) noexcept(false);
```

 * 所有地方需要使用 FatMalloc/FatFree/FatNew/FatDelete
 * 只捕获所有自己项目中的内存分配，类似 IDirectTexture9 内部分配的内部，不管

```C++
void* p = FatMalloc(10);
*(char*)p = 1;

void* p1 = FatRealloc(p, 15);
*(char*)p1 = 2;

class MyClass
{
public:
    MyClass(int v) { value_ = v; }

    void Foo() {}

private:
    int value_;
};
MyClass* my = FatNew(MyClass, 2);
my->Foo();
```

运行结果

```
<MemCheck>: Detect Memory Leak
<MemCheck>: ...\main.cpp(60) at 0x00000278ED632690, 4 bytes
<MemCheck>: ...\main.cpp(46) at 0x00000278ED632660, 15 bytes
```

[1]:https://www.flipcode.com/archives/Detecting_Memory_Leaks.shtml
[2]:https://stackoverflow.com/questions/58694487/no-matching-operator-delete-found-memory-will-not-be-freed-if-initialization-th
[3]:https://github.com/kasicass/fatdemo/blob/master/Src/Framework/Kernel/Common/Memory.h
