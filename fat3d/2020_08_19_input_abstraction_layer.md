# Input Abstraction Layer

管理键盘、鼠标、手柄等输入设备。

 * IInput - singleton input manager, 提供给其它模块调用
 * IInputDevice - 每个具体的 device，需要实现此接口，并注册到 IInput 中


## Class Hierarchy

### IInput

 * DXInput - DirectInput implementation
 * LinuxInput - SDL-based implementation

```
       IInput
         |
    +----+----+
    |         |
 DXInput  LinuxInput
```

### IInputDevice

Windows

 * DXInputDevice - base class wrappering DirectInput
 * Keyboard - DirectInput keyboard
 * Mouse - DirectInput mouse
 * XInputDevice - XInput XBOX game controller

Linux

 * LinuxInput - keyboard/mouse event using SDL

```
                     IInputDevice
                          |
         +----------------+-----------------+
         |                |                 |
    DXInputDevice    XInputDevice      LinuxInput
         |
    +----+----+
    |         |
Keyboard    Mouse
```

### Usage

```C++
class MyInputHandler : public IInputEventListener
{
	virtual bool OnInputEvent(const SInputEvent& event) override
	{
		if (event.keyId == eKey_F1)
			// do sth
	}
};

MyInputHandler myHandler;
pInput->AddEventListener(&myHandler);
```


## DirectInput & XInput

 * 现有 DirectInput，后有 XInput。XInput 只负责手柄的输入。
 * [XInput and DirectInput][1]
 * [DirectInput Docs][2]
 * [XInput Docs][3]

### DirectInput Cooperative Levels

 * [Cooperative Levels][4]
 * 使用 non-exclusive & foreground，保证只有 wnd foreground 的时候才会收到消息

```C++
if (!CreateDirectInputDevice(&c_dfDIMouse2, DISCL_NONEXCLUSIVE | DISCL_FOREGROUND, 4096))
    return false;
```


[1]:https://docs.microsoft.com/en-us/windows/win32/xinput/xinput-and-directinput
[2]:https://docs.microsoft.com/en-us/previous-versions/windows/desktop/ee416842(v=vs.85)
[3]:https://docs.microsoft.com/en-us/windows/win32/xinput/xinput-game-controller-apis-portal
[4]:https://docs.microsoft.com/en-us/previous-versions/windows/desktop/ee416848(v=vs.85)
