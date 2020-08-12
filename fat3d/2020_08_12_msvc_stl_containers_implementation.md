# VS2017 C++ STL Containers Implementation

为了精确统计 stl containers 的内存占用，把 msvc 中常用的容器都研究了一下。文章中的"内存消耗"，都是容器动态分配的内存。

常用容器

 * [std::vector][2]
 * [std::list][3]
 * [std::deque][4]
 * [std::map][5]
 * [std::set][6]
 * [std::unordered_map][7]
 * [std::unordered_set][8]


## std::vector<T>

 * container, _Container_alloc, _Container_val
 * 所有容器，基本都是这个层次关系
 * 数据结构如下

```C++
class _Vector_val
{
    pointer _Myfirst; // pointer to beginning of array
    pointer _Mylast;  // pointer to current end of sequence
    pointer _Myend;   // pointer to end of array
};

class _Vector_alloc
{
    // _Alty = type allocator, sizeof(_Alty) == 0
    // 实际数据结构由 _Vector_val 决定
    _Compressed_pair<_Alty, _Vector_val<_Val_types>> _Mypair;
};

class vector : public _Vector_alloc
{
};
```

 * vector 很简单，array of T，动态扩容
 * 扩容策略：newCapacity = oldCapacity + oldCapacity/2
 * 注意：不做 [shrink_to_fit()][1]，永远不释放内存
 * 
 * _Myfirst = T0
 * _Mylast  = _Myfirst + vector::size()
 * _Myend   = _Myfirst + vector::capacity()

```
+----+----+----+----+-----+----+
| T0 | T1 | T2 | T3 | ... | Tn |
+----+----+----+----+-----+----+
```

内存消耗

```C++
sizeof(T) * vector::capacity()
```


## std::list<T>

 * 链表，数据结构如下

```C++
class _List_node
{
    _Nodeptr _Next;
    _Nodeptr _Prev;
    _Value_type _Myval;
};

class _List_val
{
    _Nodeptr _Myhead;  // pointer to head node
    size_type _Mysize; // number of elements
};

class _List_alloc
{
    // _Alnode = node allocator
    _Compressed_pair<_Alnode, _List_val<_Val_types>> _Mypair;
};

class _List_buy : public _List_alloc
{
    // just add some functions
};

class list : public _List_buy
{    
};
```
 * 标准的双向链表实现
 * 利用 _Myhead 多一个 dummy node 的额外开销，来实现 list::end()
 * 
 * list._Myhead->_Next = first element
 * list._Myhead->_Prev = last element

```
       list._Myhead
             \
              V
             +------+     +------+     +------+     +------+
last one <-- | prev | <-- | prev | <-- | prev | <-- | prev |
             +------+     +------+     +------+     +------+
             | next | --> | next | --> | next | --> | next | --> list._Myhead
             +------+     +------+     +------+     +------+
             |  val |     |  val |     |  val |     |  val |
             +------+     +------+     +------+     +------+
```

内存消耗

```C++
sizeof(_List_node) * (list::size() + 1)
```


## std::deque<T>

```C++
#define _DEQUEMAPSIZ    8    /* minimum map size, at least 1 */
#define _DEQUESIZ    (sizeof (value_type) <= 1 ? 16 \
    : sizeof (value_type) <= 2 ? 8 \
    : sizeof (value_type) <= 4 ? 4 \
    : sizeof (value_type) <= 8 ? 2 \
    : 1)    /* elements per block (a power of 2) */

class _Deque_val
{
    _Mapptr   _Map;     // pointer to array of pointers to blocks
    size_type _Mapsize; // size of map array, zero or 2^N
    size_type _Myoff;   // offset of initial element
    size_type _Mysize;  // current length of sequence
};

class _Deque_alloc
{
    _Compressed_pair<_Alty, _Deque_val<_Val_types>> _Mypair;
};

class deque : public _Deque_alloc
{
};

// extra bytes
struct _Container_proxy
{
    const _Container_base12* _Mycont;
    _Iterator_based12* _Myfirstiter;
};
```

 * _Map 一个 block 的数组
 * 一个 block 保存 1 ~ 16 个元素 (_DEQUESIZ)
 * 根据 _DEQUESIZ 的设计，deque 不适合保存 sizeof 过大的数据
 * 
 * deque 还额外有一个 _Container_proxy。其他容器 debug build 也会有 _Container_proxy
 * 但 deque 的 release build 也会有（怀疑是bug）
 * 
 * 对于需要大量 push_back(), push_front() 的行为，deque 比较快
 * 下面是一个模拟的数据
 * sizeof(T) == 4
 * _Mapsize = 8
 * _Myoff = 2
 * _Mysize = 5

```
_Map
  +---+---+---+---+---+---+---+---+
  | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
  +---+---+---+---+---+---+---+---+
   /      \
  V        V
+----+     +----+ 
| .. |     | T2 |
+----+     +----+
| .. |     | T3 |
+----+     +----+
| T0 |     | T4 |
+----+     +----+
| T1 |     | .. |
+----+     +----+
```

内存消耗

```C++
sizeof(void*) * _Mapsize
  + num_of_not_null_map_slots * (sizeof(T) * _DEQUESIZ)
  + sizeof(_Container_proxy)
```


## std::map<K, V>

```C++
struct _Tree_node
{
    _Nodeptr _Left;     // left subtree, or smallest element if head
    _Nodeptr _Parent;   // parent, or root of tree if head
    _Nodeptr _Right;    // right subtree, or largest element if head
    char _Color;        // _Red or _Black, _Black if head
    char _Isnil;        // true only if head (also nil) node
    _Value_type _Myval; // the stored value, unused if head
};

class _Tree_val
{
    _Nodeptr _Myhead;  // pointer to head node
    size_type _Mysize; // number of elements
};

class _Tree_comp_alloc
{
    _Compressed_pair<key_compare,
        _Compressed_pair<_Alnode, _Tree_val<_Val_types>>> _Mypair;
};

class _Tree : public _Tree_comp_alloc
{
    // ordered red-black tree for map/multimap/set/multiset
};

class map : public _Tree
{
};
```

 * 标准的 red-black tree 实现
 * 多一个 dummy head node
 * _Tree_node._Myval = std::pair<K,V>

内存消耗

```C++
sizeof(_Tree_node) * (map.size() + 1)
```


## std::set<T>

 * 结构和 map 类似
 *  _Tree_node._Myval = K

内存消耗

```C++
sizeof(_Tree_node) * (set.size() + 1)
```


## std::unordered_map<K, V>

```C++
class _Hash
{
    // hash table -- list with vector of iterators for quick access

    _Traits _Traitsobj; // traits to customize behavior
    _Mylist _List;      // list of elements, must initialize before _Vec
    _Myvec _Vec;        // vector of list iterators, begin() then end()-1
    size_type _Mask;    // the key mask
    size_type _Maxidx;  // current maximum key value
};

class unordered_map : public _Hash
{
};
```

 * 标准 HashTable 实现
 * 所有元素放在 _List 中
 * _Vec 作为 HashTable 的 buckets，保存着 _List 的 iterator
 * _Vec[i] 和 _Vec[i+1] 为一组，对应 _List 中的一段数据
 * Hash(key) 找到 i，然后遍历 _List 中 _Vec[i] ~ _Vec[i+1] 所有的元素即可

```
        one bucket
_Vec     /
    /-------\
    +---+---+---+---+
    | 0 | 1 | 2 | 3 |
    +---+---+---+---+
      |   \
      |    --------------
_List V                  V
    +----+    +----+    +----+    +----+
    | V1 | -> | V2 | -> | V3 | -> | V4 | ...
    |    | <- |    | <- |    | <- |    |
    +----+    +----+    +----+    +----+
```

内存消耗

```C++
// _List_node = { void* prev; void* next; std::pair<K,V> elem; }
sizeof(_List::iterator) * (map.bucket_count()*2)
  + sizeof(_List_node) * (map.size() + 1)
```


## std::unordered_set<T>

 * 结构和 unordered_map 类似

内存消耗

```C++
// _List_node = { void* prev; void* next; T elem; }
sizeof(_List::iterator) * (map.bucket_count()*2)
  + sizeof(_List_node) * (map.size() + 1)
```


[1]:https://en.cppreference.com/w/cpp/container/vector/shrink_to_fit
[2]:https://en.cppreference.com/w/cpp/container/vector
[3]:https://en.cppreference.com/w/cpp/container/list
[4]:https://en.cppreference.com/w/cpp/container/deque
[5]:https://en.cppreference.com/w/cpp/container/map
[6]:https://en.cppreference.com/w/cpp/container/set
[7]:https://en.cppreference.com/w/cpp/container/unordered_map
[8]:https://en.cppreference.com/w/cpp/container/unordered_set
