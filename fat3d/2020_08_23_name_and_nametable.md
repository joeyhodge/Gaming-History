# Name and NameTable

## 设计思路

引擎中一些 string，会长期保存在内存，希望其具有

 * 只保存一份数据
 * 快速 compare，快速 copy

典型用例。美术资源中互相索引的资源路径，比如：

 * Textures/maps/block.dds
 * Objects/maps/valley/rock.model


## 使用范例

 * Name 和普通 string 一样，copy / compare

```C++
Name a = "Textures/maps/block.dds"
Name b = a;

if (a == b)
    ...

if (b.Empty())
    ...
```


## 实现细节

### 代码讲解

 * Name，外部使用的 class
 * Name 中只保存一个 pointer 指向 NameMap 中的数据
 * Name.str_ 指向 SNameEntry.data
 * Name.str_ 就是 string 本身，同时又可以用来 compare，因为 string 只有一份

```C++
enum ENameFlag
{
    eNameFlag_OnlyFind,
};

class Name
{
public:
    Name();
    Name(const Name& rhs);
    Name(const char* s);
    Name(const char* s, ENameFlag onlyFind);
    ~Name();

    ...

public:
    // std::unordered_map helpers
    struct hash
    {
        size_t operator()(const Name& key) const;
    };

private:
    const char* str_;
};
```

 * INameTable，实际 string 的数据池，refcount 管理 string
 * FatSystem.dll 中实现 INameTable

```C++
struct INameTable
{
    struct SNameEntry
    {
        int refcount;           // Reference count of this string.
        int length;             // Current length of string.
        // char data[length+1]  // Here in memory starts character buffer of size length+1.
    };
    ...
};

class NameTable : public INameTable
{
    ...
private:
    typedef std::unordered_map<const char*, SNameEntry*, stl::hash_cstring<const char*>, stl::equal_stricmp<const char*>> NameMap;
    NameMap nameMap_;
};
```

### 原理图

```
                     NameVar1    NameVar2
                        |            |
                        +-------+----+
                                |
                                V
                       +---+---+-------------------------+
   +--------+     /->  | R | L | Textures/maps/block.dds |
   | Name   | ---/     +---+---+-------------------------+
   |  Table |                
   +--------+
         \      +---+---+--------------------------------+
          \---> | R | L | Objects/maps/valley/rock.model |
                +---+---+--------------------------------+
                         ^
                         |
                      NameVar3
```

### 关于 static Name var

 * 对于 static Name var，因为有可能在 NameTable 构建之前初始化
 * 在 NameTable 析构之后析构，所以要特别注意初始化方式
 * 
 * 以 Texture::s_texClassName 为例，要保证在 NameTable 构建之后，显示调用 InitNames()

```C++
class Texture
{
public:
    static const Name& ClassName();
    static void InitNames();

private:
    static Name s_texClassName;
    ...
};


Name Texture::s_texClassName; // = Name("Texture");

const Name& Texture::ClassName()
{
    return s_texClassName;
}

void Texture::InitNames()
{
    s_texClassName = "Texture";
}
```

 * static Name var 的析构，由 Name::~Name() 中来保证

```C++
Name::~Name()
{
    // for static var, NameTable maybe have been destroyed before ~Name()
    // e.g. "static Name Texture::s_texClassName"
    //
    // we make gEnv->pNameTable = NULL, meaning NameTable has been destroyed.
    INameTable* pNameTable = GetNameTableNoCheck();
    if (pNameTable)
        pNameTable->StringRelease(str_);
}
```
