# ISizer - Memory Statistics

精准统计 C++ 内存消耗，一直是个难题。

 * 方法一：用 [tcmalloc][1] 之类，统计所有 malloc/new 的地方
 * 方法二：由每一个 class 自己汇报用了多少内存

方法一在 Windows 下，有时由于抓取的数据不精准，导致分析不准确。大部分时候还是准确的。

方法二在统计 STL containers 动态分配的内存时，会比较麻烦。

ISizer 实现了方法二，并精准统计 STL containers 的消耗。


## ISizer usage

 * 每个类要实现 GetMemoryUsage() 函数
 * FAT_SIZER_COMPONENT_NAME() 标记类，用于统计
 * pSizer->AddThis(this) 会统计 this 对应的类的大小 sizeof(T)
 * 如果已经统计过 this, pSizer->AddThis() 返回 false

```C++
struct MyPOD
{
    int value;
    float fvalue;
    double dvalue;
    char s[7];

    MyPOD()
    {
        value = rand();
    }
};

class Basic : public RefCounter
{
public:
    Basic()
    {
        for (int i = 0; i < 79; ++i)
            arrPods_.emplace_back(MyPOD());
    }

    void GetMemoryUsage(ISizer* pSizer) const
    {
        FAT_SIZER_COMPONENT_NAME(pSizer, "Basic");
        if (!pSizer->AddThis(this))
            return;

        pSizer->AddContainer(arrPods_);
    }

private:
    std::vector<MyPOD> arrPods_;
};
typedef TSmartPtr<Basic> BasicPtr;

class Complex
{
public:
    Complex()
    {
        BasicPtr b1 = new Basic();
        BasicPtr b2 = new Basic();
        mapBasics_["1"] = MapItem(1, b1);
        mapBasics_["2"] = MapItem(1, b1);
        mapBasics_["key_long_enough_go_go_go_3"] = MapItem(1, b1);
        mapBasics_["key_long_enough_go_go_go_key_long_enough_go_go_go_4"] = MapItem(2, b2);
        mapBasics_["key_long_enough_go_go_go_key_long_enough_go_go_go_key_long_enough_go_go_go_5"] = MapItem(2, b2);
    }

    void GetMemoryUsage(ISizer* pSizer) const
    {
        FAT_SIZER_COMPONENT_NAME(pSizer, "Complex");
        if (!pSizer->AddThis(this))
            return;

        // 注意，it->first 是 std::string, it->second.basic 是 class Basic
        // 两者都有动态内存分配，需要调用 AddObject() 统计之
        pSizer->AddContainer(mapBasics_);        // MapItem.value is calcuated here
        for (auto it = mapBasics_.cbegin(); it != mapBasics_.cend(); ++it)
        {
            pSizer->AddObject(it->first);
            pSizer->AddObject(it->second.basic); // just calculate MapItem.basic
        }
    }

private:
    struct MapItem {
        int value;
        BasicPtr basic;

        MapItem() {}
        MapItem(int v, BasicPtr pBasic) : value(v), basic(pBasic) {}
    };

    std::map<std::string, MapItem> mapBasics_;
};
```

统计结果

```
Memory Stats: KB
           TOTAL   partial   count
TOTAL   :   5.34          
 Complex:   5.34      0.80       1
 .Basic :             4.53       2
```


## ISizer 实现原理

 * ISizer 只负责统计，输出统计结果，可以再独立用一个类实现

### 接口设计

```C++
struct ISizer
{
    // id 一般就是 this pointer，用来唯一标记此对象，防止重复统计
    virtual bool RecordObject(const void* id, size_t sizeBytes, int count = 1) = 0;

    // AddThis() 给类使用的
    template <typename T>
    bool AddThis(const T* id)
    {
        return RecordObject(id, sizeof(T));
    }

    // 其它 AddObject() 都没映射上，调用 GetMemoryUsage() 函数
    template <typename T>
    void AddObject(const T& obj)
    {
        obj.GetMemoryUsage(this);
    }

    // 如果是 pointer，调用对应的其他 AddObject() 函数
    template <typename T>
    void AddObject(T* pObj)
    {
        if (pObj)
            this->AddObject(*pObj);
    }

    // 对于基础类型，AddThis() 时已经被统计入 sizeof(T)，则 AddObject(basicType) 忽略就好
    // 防止产生编译错误
    void AddObject(const char&) {}
    void AddObject(const unsigned char&) {}
    void AddObject(const short&) {}
    void AddObject(const unsigned short&) {}
    void AddObject(const int&) {}
    void AddObject(const unsigned int&) {}
    void AddObject(const long&) {}
    void AddObject(const unsigned long&) {}
    void AddObject(const long long&) {}
    void AddObject(const unsigned long long&) {}
    void AddObject(const float&) {}
    void AddObject(const double&) {}
    void AddObject(const bool&) {}

    // overloads for TSmartPtr
    template <typename T>
    void AddObject(const TSmartPtr<T>& obj) { this->AddObject(obj.Get()); }

    // 对于 STL containers，统一使用 AddContainer() 函数
    template <typename T, typename Alloc> void AddContainer(const std::vector<T, Alloc>& vec);
    template <typename T, typename Alloc> void AddContainer(const std::list<T, Alloc>& list);
    ...

    // 如果 container 中的元素，都是 pointer 类型(比如 SmartPtr)，可以直接用 AddContainerObjects()
    template <typename T, typename Alloc>
    void AddContainerObjects(const std::vector<T, Alloc>& vec)
    {
        for (auto it = vec.cbegin(); it != vec.cend(); ++it)
        {
            this->AddObject(*it);
        }
    }
};
```

### MyClass::GetMemoryUsage() 的标准实现

 * 根据情况灵活实现
 * AddThis() / AddObject() / AddContainer() / AddContainerObjects()
 * AddContainer() 命名本也可以统一为 AddObject()，但 STL Containers 比较特殊，独立一个名字会更好

```C++
void MyClass::GetMemoryUsage(ISizer* pSizer) const
{
    FAT_SIZER_COMPONENT_NAME(pSizer, "MyClass");
    if (!pSizer->AddThis(this))
        return;

    pSizer->AddContainer(arrNames_);

    pSizer->AddContainer(arrComplex_);
    pSizer->AddContainerObjects(arrComplex_);

    pSizer->AddObject(subObj_);
}
```


### 统计 STL Containers

精准统计 STL containers 内存消耗的原理，就是理解每一个编译器 STL containers 的实现。

 * [MSVC STL Containers Implementation][2]
 * [Clang STL Containers Implementation][3]
 * [GCC STL Containers Implementation][4]

以 std::map 为例：

```C++
template <typename K, typename T, typename Comp, typename Alloc>
void AddContainer(const std::multimap<K,T,Comp,Alloc>& map)
{
#if defined(FAT_COMPILER_MSVC)

    struct Dummy { void* a; void* b; void* c; char d; char f; K k; T t; };

#    if defined(FAT_DEBUG_BUILD)
    const size_t extraBytes = sizeof(std::_Container_proxy) + sizeof(Dummy);
#    else
    const size_t extraBytes = sizeof(Dummy); // dummy head node
#    endif

    if (!map.empty())
        this->RecordObject(&(*map.cbegin()), map.size() * sizeof(Dummy) + extraBytes, 0);
    else
        this->RecordObjectSizeOnly(extraBytes);

#else

    struct Dummy { void* a; void* b; void* c; char d; char f; K k; T t; };
    if (!map.empty())
        this->RecordObject(&(*map.cbegin()), map.size() * sizeof(Dummy), 0);

#endif
}
```


## Pros & Cons

Pros

 * 精准统计内存
 * 可以看到类和子类内存消耗的详细关系

Cons

 * 每一个类需要实现 GetMemoryUsage() 函数
 * 使用者需要理解 stack & heap 在内存申请上的区别，否则容易写错统计代码，导致结果出错


[1]:https://github.com/gperftools/gperftools
[2]:https://github.com/kasicass/blog/blob/master/fat3d/2020_08_12_msvc_stl_containers_implementation.md
[3]:https://github.com/kasicass/blog/blob/master/fat3d/2020_08_13_clang_stl_containers_implementation.md
[4]:https://github.com/kasicass/blog/blob/master/fat3d/2020_08_13_gcc_stl_containers_implementation.md
