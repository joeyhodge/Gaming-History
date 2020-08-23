# [win32] CrashReport机制 (2) -- 给dump弄个好名字

可以通过编译时间，给dmp弄个名字。比如：ProductName-20120724-18:05:37.dmp

然后编译好的 .exe, .pdb 文件也根据时间放到不同的目录，备用。

制作 compileTimestamp

```C++
#include <stdio.h>
#include <assert.h>
#include <string>

// C Standard
// __DATE__ == "Mmm dd yyyy"
// __TIME__ == "hh:mm:ss"
static int getMonth(const char *s)
{
    static const char * months[] = {
        "Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
    };

    for (int i = 0; i < 12; i++)
    {
        if (strcmp(months[i], s) == 0)
            return i+1;
    }

    assert(0 && "why?");
    return 0;
}

static char *generateCompileTimestamp()
{
    static char compileTime[80];
    std::string date = __DATE__;
    int month = getMonth(date.substr(0, 3).c_str());
    int day   = atoi(date.substr(4, 2).c_str());
    std::string year = date.substr(7, 4);
   
    sprintf(compileTime, "%s%02d%02d-%s", year.c_str(), month, day, __TIME__);
    return compileTime;
}

// 20110205-17:03:29
const char *const compileTime = generateCompileTimestamp();
```

这里再带上 crash 所在的位置，来个唯一名字。

我编译的 32-bit exe，如果在 64-bit system 下跑在 wow64 环境下，只有 ExceptionAddress 的 0xFFFF 是不变的，0xFFFF0000会变化。

```C++
#include <stdio.h>
#include <Windows.h>

LONG WINAPI myExceptionFilter(struct _EXCEPTION_POINTERS* ExceptionInfo)
{
    char fileName[256];
    DWORD addr = DWORD(ExceptionInfo->ExceptionRecord->ExceptionAddress) & 0xFFFF;
    sprintf(fileName, "ProductName-%s-%04X.dmp", compileTime, addr);
    printf("dmpfile: %s\n", fileName);

    return EXCEPTION_EXECUTE_HANDLER;
}

extern const char *const compileTime;
int main()
{
    SetUnhandledExceptionFilter(myExceptionFilter);

    int *p = 0;
    *p = 1;

    return 0;
}
```

如何获取准确的 crash 地址，上面的方法可行。更好的方法应该是 StackWalk64() 等函数来遍历栈，根据栈地址算一个更准确的值，保证栈也是唯一的。不过我测试了有问题，没有深究。
 * [http://stackoverflow.com/questions/590160/how-to-log-stack-frames-with-windows-x64][1]

[1]:http://stackoverflow.com/questions/590160/how-to-log-stack-frames-with-windows-x64
