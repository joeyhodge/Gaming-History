# [game-dev] 关于hotfix

hotfix, 热更新, 热部署, 动态代码更新

对运行中的系统，实时地替换某个模块（某个函数）的代码。

## 关于C/C++

对于 C/C++，函数入口在编译后，固定为一段内存地址。想要hotfix，则需要“每次调用此函数，先通过func_map做一个查表”这样来做到。比如：

```C
void foo();
func_map["foo"] = foo;

void bar()
{
    myfoo = func_map["foo"];
    myfoo();
}
```

将 foo() 函数放在独立DLL中，更新此DLL文件，然后通知主程序重新load此DLL，并更新func_map，则达到了动态修改代码的目的。

这是根据原理推导出来的方法，但我未在项目中实际用过。

## 关于脚本语言(lua/python)

来说说脚本语言的hotfix，这是我们每天都要用到的功能。

脚本中跨模块调用，都是"查表找到具体的函数，再调用之"的方式（所以才动态嘛），很适合来做 hotfix

### 关于python

编辑 foo.py 文件

```python
class Foo(object):
  def gogo(self):
    print 'Foo.gogo 111'

>> import foo
>> f = foo.Foo()
>> f.gogo()
Foo.gogo 111
```

修改 foo.py

```python
class Foo(object):
  def gogo(self):
    print 'Foo.gogo 222'

>> reload(foo)
>> f.__class__ = foo.Foo
>> f.gogo()
Foo.gogo 222
```

这里要注意，要hotfix生效，不要直接引用 instance 的 class method，比如：

```
>> callback = f.gogo
>> callback()
Foo.gogo 222
```

修改 foo.py

```python
class Foo(object):
  def gogo(self):
    print 'Foo.gogo 333'

>> reload(foo)
>> callback()
Foo.gogo 222
```

所以，要这样，才能保证hotfix。

```
>> callback = lambda: f.gogo()
```

同理，对于普通函数，也不要用局部变量引用起来

```
>> foo.somefunc()
>> callback = lambda: foo.somefunc()
>> callback()
```

不能

```
>> callback = foo.somefunc
>> callback()
```

### 关于lua

lua 没有类的概念，所有的替换，其实就是替换模块中的函数。

编辑 foo.lua

```lua
module("foo", package.seeall)

function hello()
    print("hello 111")
end

> require("foo")
> foo.hello()
hello 111
```

修改 foo.lua

```lua
module("foo", package.seeall)

function hello()
    print("hello 222")
end

> package.loaded["foo"] = nil
> require("foo")
> foo.hello()
hello 222
```

一样的，不要直接引用模块中的函数。

如果通过 metatable 设计了一个类机制，自己考虑好，可以保证更新到位。

## hotfix的终极解法(erlang)

以上脚本在设计之初，是没有考虑hotfix的，所以在实践hotfix时，需要写代码同学注意一些问题（不能直接引用模块中的函数）

erlang 这块是彻底解决了这个问题，包括变量都是不能二次赋值的，在并发和热更新方面都无比顺畅。

停止模块执行 => 更换模块代码 => 调用code_change转换模块老的数据 => 恢复模块的执行
