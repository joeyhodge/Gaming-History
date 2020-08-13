# GCC STL Containers Implementation

既然研究了 MSVC STL 实现，顺带把 GCC 的也看了一遍。

 * debian 9.5 - gcc 6.3.0

常用容器

 * [std::vector][2]
 * [std::list][3]
 * [std::deque][4]
 * [std::map][5]
 * [std::set][6]
 * [std::unordered_map][7]
 * [std::unordered_set][8]


## std::vector<T>

 * /usr/include/g++/bits/stl_bvector.h

```C++
struct _Vector_base
{
	struct _Vector_impl
	{
		_Tp* _M_start;
		_Tp* _M_finish;
		_Tp* _M_end_of_storage;
	};

	_Vector_impl _M_impl;
};

class vector : protected _Vector_base
{
};
```

 * vector 很简单，array of T，动态扩容
 * 扩容策略：newCapacity = oldCapacity*2 (TODO)
 * 注意：不做 [shrink_to_fit()][1]，永远不释放内存
 * 
 * _M_start          = T0
 * _M_finish         = _M_start + vector::size()
 * _M_end_of_storage = _M_start + vector::capacity()

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

```C++
struct _List_node_base
{
	_List_node_base* _M_next;
	_List_node_base* _M_prev;
};

template <typename _Tp>
struct _List_node : public _List_node_base
{
	_Tp _M_data;
};

class _List_base
{
	struct _List_impl
	{
		_List_node_base _M_node;
	};

	_List_impl _M_impl;
};

class list : protected _List_base
{
};
```

 * 标准的双向链表实现
 * list._M_impl._M_node 作为 dummy node
 * 
 * list._M_impl._M_node._M_next = first element
 * list._M_impl._M_node._M_prev = last element

```
list._M_impl._M_node                                     last one
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
sizeof(_List_node) * list.size()
```


## std::deque<T>

 * /usr/include/c++/6/bits/stl_deque.h

```C++
inline size_t
__deque_buf_size(size_t __size)
{
	return __size < 512 ? size_t(512 / __size) : size_t(1);
}

struct _Deque_iterator
{
	_Tp* _M_cur;
	_Tp* _M_first;
	_Tp* _M_last;
	_Map_pointer _M_node;
};

class _Deque_base
{
	struct _Deque_impl
	{
		_Tp**    _M_map;
		size_t   _M_map_size;
		iterator _M_start;
		iterator _M_finish;
	};

	_Deque_impl _M_impl;
};

class deque : protected _Deque_base
{
};
```

 * _M_Map 一个 block 的数组
 * sizeof(T) < 512，一个 block 大小为 512 bytes，保存 N 个元素 ( N = __deque_buf_size(sizeof(T)) )
 * sizeof(T) >= 512，则一个 block 只保存一个元素
 * 
 * 对于需要大量 push_back(), push_front() 的行为，deque 比较快
 * 下面是一个模拟的数据
 * sizeof(T) == 4
 * _M_map_size = 8
 * _M_start    = iterator point to T0
 * _M_finish   = iterator point to T4

```
_M_map
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
sizeof(void*) * _M_map_size
  + num_of_not_null_map_slots * (sizeof(T) * __deque_buf_size(sizeof(T)))
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
