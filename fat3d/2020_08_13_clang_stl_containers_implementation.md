# Clang STL Containers Implementation

既然研究了 MSVC STL 实现，顺带把 clang(llvm) 的也看了一遍。

 * OpenBSD 6.4 - LLVM/Clang 6.0.0

常用容器

 * [std::vector][2]
 * [std::list][3]
 * [std::deque][4]
 * [std::map][5]
 * [std::set][6]
 * [std::unordered_map][7]
 * [std::unordered_set][8]


## std::vector<T>

 * /usr/include/c++/v1/vector

```C++
class __vector_base
{
	pointer __begin_;
	pointer __end_;
	__compressed_pair<pointer, allocator_type> __end_cap_; // capacity
};

class vector : private __vector_base
{
};
```

 * vector 很简单，array of T，动态扩容
 * 扩容策略：newCapacity = oldCapacity*2
 * 注意：不做 [shrink_to_fit()][1]，永远不释放内存
 * 
 * __begin_   = T0
 * __end_     = __begin_ + vector::size()
 * __end_cap_ = __begin_ + vector::capacity()

```
+----+----+----+----+-----+----+
| T0 | T1 | T2 | T3 | ... | Tn |
+----+----+----+----+-----+----+
```

内存消耗

```C++
sizeof(T) * vector.capacity()
```


## std::list<T>

 * /usr/include/c++/v1/list

```C++
struct __list_node_base
{
	__link_pointer __prev_;
	__link_pointer __next_;
};

struct __list_node : public __list_node_base
{
	_Tp __value_;
};

class __list_imp
{
	__node_base __end_;                                           // dummy node
	__compressed_pair<size_type, __node_allocator> __size_alloc_; // size of list
};

class list : private __list__imp
{
};
```

 * 标准的双向链表实现
 * list.__end_ 作为 dummy node
 * 
 * list.__end_._M_next = first element
 * list.__end_._M_prev = last element

```
       list.__end_                                    last one
               \                                       /
             +------+     +------+     +------+     +------+
last one <-- | prev | <-- | prev | <-- | prev | <-- | prev |
             +------+     +------+     +------+     +------+
             | next | --> | next | --> | next | --> | next | --> list._M_impl._M_node
             +------+     +------+     +------+     +------+
                          |  val |     |  val |     |  val |
                          +------+     +------+     +------+
```

内存消耗

```C++
sizeof(__list_node) * list.size()
```


## std::deque<T>

 * /usr/include/c++/v1/list
 * /usr/include/c++/v1/__split_buffer

```C++
struct __split_buffer
{
	typedef T* pointer;

	pointer                                    __first_;
	pointer                                    __begin_;
	pointer                                    __end_;
	__compressed_pair<pointer, allocator_type> __end_cap_;
};

template <class _ValueType, class _DiffType>
struct __deque_block_size {      
	static const _DiffType value = sizeof(_ValueType) < 256 ? 4096 / sizeof(_ValueType) : 16;
};  

class __deque_base
{
	static const difference_type __block_size;

	typedef T* pointer;
	typedef __split_buffer<pointer, __pointer_allocator> __map;

	__map                                        __map_;
	size_type                                    __start_;
	__compressed_pair<size_type, allocator_type> __size_;  // element count of deque
};

class deque : private __deque_base
{
};
```

 * sizeof(T) < 256，一个 block 固定大小 4096，__deque_block_size() 返回 count of block elements
 * sizeof(T) >= 256，一个 block 有 16 个元素
 * 
 * __split_buffer::pointer 相对于 deque 是 T**
 * 
 * deque.__map_ 维护着 block-pointer array
 * __map_.__begin_ ~ __end_ 之间是有 block 的，其他位置只有 null block-pointer
 * 
 * deque.__start_ = 2, T0 的位置

```
__map_
__first_   __begin_   __end_         __end_cap_
      \        |        /               /
       +---+---+---+---+---+---+---+---+
       | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
       +---+---+---+---+---+---+---+---+
                /     \
               V       V
            +----+    +----+ 
            | .. |    | T2 |
            +----+    +----+
            | .. |    | T3 |
            +----+    +----+
            | T0 |    | T4 |
        	+----+    +----+
            | T1 |    | .. |
            +----+    +----+
			block 0   block 1
```

内存消耗

```
sizeof(void*) * deque.__map_.capacity()
  + deque.__map_.size() * (sizeof(T) * deque::__block_size)
```

[1]:https://en.cppreference.com/w/cpp/container/vector/shrink_to_fit
[2]:https://en.cppreference.com/w/cpp/container/vector
[3]:https://en.cppreference.com/w/cpp/container/list
[4]:https://en.cppreference.com/w/cpp/container/deque
[5]:https://en.cppreference.com/w/cpp/container/map
[6]:https://en.cppreference.com/w/cpp/container/set
[7]:https://en.cppreference.com/w/cpp/container/unordered_map
[8]:https://en.cppreference.com/w/cpp/container/unordered_set
[9]:https://en.cppreference.com/w/cpp/string/basic_string/shrink_to_fit
