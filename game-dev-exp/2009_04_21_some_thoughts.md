# [game-dev] 一点感想

下午听了公司某位大大（[云风][1]）讲解他们设计的引擎，其中对某些设计的一点感想，记录之。

## 关于 GUI

我们的游戏，最终还是要作为一个 window(窗体) 呈现给玩家，所有的操作，其实还是2D的，所以在整个窗体结构的设计上，从上到下，可以把 window 拆分成三个层次，cursor / 2DGUI / 3D。

cursor layer，我们认为鼠标一定在所有显示内容的上面，所有在这一层的 node(image, ani) 都跟随这鼠标的坐标点(root) 运动即可。

2DGUI layer，这里就是我们传统的UI系统所在，button、edit 等，该有啥，就有啥。

3D layer，最地下一层，整个 layer 与窗体大小一样大，其实就是 3D viewport 最后渲染到这一层上。对于鼠标事件，2DGUI layer 没有接收到的消息，都发往 3D layer

整个层次结构的设计，是以2D框架为主，3D作为2D的一部分来看待的。另一种做法，就是把 2DGUI 做成 3D rendering 过程的一部分，比如目前我们使用的引擎的UI系统，就是在绘制完3D场景后，2D UI也是通过矩阵变化绘制到屏幕上，整个逻辑于3D绑得比较死。

对于 2DGUI layer 本身，还可以分为几个层，比如 top/normal/bottom。对于单独的一个 sub layer，可以进行整体显示/隐藏。大部分的控件，都可以放在 normal，比如 按下 Alt + T 组队状态下，要求把大部分 UI 都隐藏起来，就可以通过隐藏整个 normal layer 来做到。

## 关于对象管理

现在的游戏，都喜欢用 C/C++ 配合 lua/python 来搭建整个逻辑，而 C/C++ 对象自然也要放到脚本层中去使用，自然也是要管理的。传统的结构：

```
         C/C++                |                lua/python
                              |
        parent wnd            |                parent wnd
         /    \               |                /         \
    child 1   child 2         |             child 1     child 2
```

无论在 C/C++, 还是 lua/python 里面，都会有 parent/child1/child2 三者之间的关系。因为一份关系，被两个地方保存起来，自然就有状态不同步的问题，比如我把 parent wnd 干掉了，如果 C/C++ 主动把 child 1, child 2 也干掉，是否 lua/python 里面还依赖这 child 1 / child 2 做点事情。如此这般，refcount 什么的一出来，逻辑就复杂了。

而另一种想法，就是所有对象的 create/del 都是由 lua/python 这一层来负责的，结构变成了这样：

```
         C/C++                |                lua/python
                              |
        parent wnd            |                parent wnd
                              |                /         \
    child 1   child 2         |             child 1     child 2
```

对于 C/C++ 它不关心 parent/child1/child2 之间的关系，三者只由 lua/python 层负责创建，关系也由其管理。最后在 lua/python 层删除 parent wnd 的时候，我们并不直接删除 child 1/ child 2，等到 lua gc 的时候，自然就会调用 C/C++ 里面的析构函数，释放内存。

简单来说，就是不要存在冗余的关系，给一个部分负责管理就好。

## 关于接口设计

同样的，C/C++ 配合 lua/python 这个的搭配，我们需要把一些函数注册给 lua/python 来使用，一般的通用的方法，就是 C/C++ 中增加了某个功能，如果 lua/python 中要使用到，把对应的函数导出到脚本中就好。久而久之，导出的函数就会有一大堆，如：

```
mymodule_create_obj()
mymodule_del_obj()
mymodule_set_obj_foo()
mymodule_get_obj_foo()
mymodule_set_obj_bar()
mymodule_get_obj_bar()
....
```

这个粘合层，就很“厚”了。

反之，我们可以通过引入一个“间接层”来把粘合层做“薄”，如：

```
mymodule_create_obj()
mymodule_del_obj()
mymodule_get_obj(obj, key)
mymodule_set_obj(obj, key, ...)
```

如此，对于一个 module，基本上只需要导出此四个函数，就足够了。具体的行为，我们可以通过 key 的不同，在 get/set 中 switch case 来完成。新加功能时，多加个 case 即可，并不需要导出接口。


[1]:https://blog.codingnow.com/
