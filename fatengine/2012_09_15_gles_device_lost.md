# Android Log & Activity Lifecycle & GLES Device Lost  

## Android Log

```
import android.util.Log;

Log.v(tag, msg), verbose
Log.d(tag, msg), debug
Log.i(tag, msg), info
Log.w(tag, msg), warning
Log.e(tag, msg), error
```

tag 就是个标签，用来过滤的，比如：

```Java
Log.d("LifeCycleActivity", "onCreate");
```

监听以 "LifeCycleActivity" 为 tag 且 >= debug 的所有 msg

```
adb logcat -s LifeCycleActivity:d
D/LifeCycleActivity(  721): onCreate
```

参考资料

 * [http://fins.iteye.com/blog/141021][1]
 * [http://blog.csdn.net/Android_Tutor/article/details/5081713][1]


## Activity Lifecycle

在 android 上，activity 就代表一个界面，一个 process 有多个、或者没有 activity。

 * onCreate(), activity 创建
 * onStart(), activity 显示出来
 * onRestart(), onStop() 之后又恢复了
 * onResume(), activity 开始接受用户输入
 * onPause(), activity 停止接受用户输入，但还可见（另一个 non-fullscreen 的 activity 在你的 activity 上面）
 * onStop(), activity 不可见
 * onDestroy(), activity 释放

来看一个完整的程序：

```
package com.kcoder.lifecycle;

import android.app.Activity;
import android.os.Bundle;
import android.util.Log;

public class LifeCycleActivity extends Activity
{
  @Override
  public void onCreate(Bundle savedInstanceState)
  {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.main);
    Log.d("LifeCycleActivity", "onCreate");
  }

  @Override
  public void onStart()
  {
    super.onStart();
    Log.d("LifeCycleActivity", "onStart");
  }

  @Override
  public void onRestart()
  {
    super.onRestart();
    Log.d("LifeCycleActivity", "onRestart");
  }
  @Override
  public void onResume()
  {
    super.onResume();
    Log.d("LifeCycleActivity", "onResume");
  }

  @Override
  public void onPause()
  {
    super.onPause();
    Log.d("LifeCycleActivity", "onPause");
  }

  @Override
  public void onStop()
  {
    super.onStop();
    Log.d("LifeCycleActivity", "onStop");
  }

  @Override
  public void onDestroy()
  {
    super.onDestroy();
    Log.d("LifeCycleActivity", "onDestroy");
  }
}
```

进程启动，看到activity，可以收到

```
D/LifeCycleActivity(  721): onCreate
D/LifeCycleActivity(  721): onStart
D/LifeCycleActivity(  721): onResume
```

来看看模拟器的按钮

![](2012_09_15_gles_device_lost_image_01.png)

点 ![](2012_09_15_gles_device_lost_image_02.png) 表示回到桌面，但当前进程 activity 并不退出，可以收到消息：

```
D/LifeCycleActivity(  646): onPause
D/LifeCycleActivity(  646): onStop
```

此时恢复进程，可以收到消息：

```
D/LifeCycleActivity(  594): onRestart
D/LifeCycleActivity(  594): onStart
D/LifeCycleActivity(  594): onResume
```

如果 ![](2012_09_15_gles_device_lost_image_03.png) 表示退出当前的 activity，可以收到消息：

```
D/LifeCycleActivity(  721): onPause
D/LifeCycleActivity(  721): onStop
D/LifeCycleActivity(  721): onDestroy
```

此时恢复进程，可以收到消息：（看起来和第一次启动进程是一样的，但此时其进程并未退出）

```
D/LifeCycleActivity(  788): onCreate
D/LifeCycleActivity(  788): onStart
D/LifeCycleActivity(  788): onResume
```

如何确定一个进程是否退出？android 是尽量不让进程退出的，除非整个系统 low memory 了，系统会主动 kill app（参考上面的 lifecycle 流程图）

可以通过长按 ![](2012_09_15_gles_device_lost_image_01.png) 看进程列表，在进程列表中向右拖这个进程，将其退出。

或者通过 shell 进入 android 的机器上

```
> adb shell
# ps
USER     PID   PPID  VSIZE  RSS     WCHAN    PC         NAME
u0_a47    788   37    173128 32952 ffffffff 40033a40 S com.kcoder.lifecycle
# kill 788
```

按 ![](2012_09_15_gles_device_lost_image_04.png) 可以打开 System Settings，设置如下选项，则按下 ![](2012_09_15_gles_device_lost_image_02.png) 的行为就和 ![](2012_09_15_gles_device_lost_image_03.png) 一样了（删除了activity）。

 * System Settings => Developer options => Don't keep activities [X]

关于 activity lifecycle，官方文档里有详细解释。

 * [http://developer.android.com/reference/android/app/Activity.html][3]
 * [http://developer.android.com/reference/android/app/Activity.html#ProcessLifecycle][4]

**这里要留意 activity 的生命周期，特别是隶属于 activity 的 static 变量**


## GLES Device Lost

好，上面是背景知识，下面来说说碰到的问题。

PC平台上，在 DX9 中，我们知道会有 device lost。当 device lost 发生后，需要我们手动恢复 D3DPOOL_DEFAULT(显存中) 类型 texture 的数据。而对于 PC 平台的 OpenGL 程序，我们不需要关心 device lost，因为 driver 帮我们管理了。

对于移动平台，资源是受限的，android 也不帮你管理这些事情，通过 OpenGL ES 创建的资源(texture, shader等)，在 activity 处于不显示状态后，这些资源会被系统自动回收的，所以在 onStop() 时要去释放所有 gles 相关资源，等 activity 恢复后，下次又用到这些资源时，程序的资源管理器要 check 资源是否还存在，不存在了，则再次创建这些资源。其实也就是移动平台上的 device lost 了。


[1]:http://fins.iteye.com/blog/141021
[2]:http://blog.csdn.net/Android_Tutor/article/details/5081713
[3]:http://developer.android.com/reference/android/app/Activity.html
[4]:http://developer.android.com/reference/android/app/Activity.html#ProcessLifecycle
