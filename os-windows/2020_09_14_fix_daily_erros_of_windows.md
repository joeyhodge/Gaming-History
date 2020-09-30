# Fix Daily Errors of Windows

 * 2020.09.30 - Fix Error IRQL_NOT_LESS_OR_EQUAL
 * 2020.09.14 - Error 997. Overlapped I/O operation is in progress


## Fix Error IRQL_NOT_LESS_OR_EQUAL

### Some Reasons

根据我机器的情况，很可能是内存的问题

 * The system file is damaged.
 * The driver is not compatible.
 * **The CPU is overheated**.
 * There is a problem with the hardware. Such as **motherboard or RAM is damaged**
 * Corrupted registry.
 * The software is not installed correctly.

启动 Windows Memory Diagnostic 检查之。

 * 运行 "mdsched"

### Reference

 * Youtube: [How to QUICKLY Fix Error IRQL_NOT_LESS_OR_EQUAL - Windows 7/8/10][4]
 * [7 Solutions to Fix IRQL_NOT_LESS_OR_EQUAL Windows 10][5]
 * [How to run Windows Memory Diagnostics Tool in Windows 10][6]


## Error 997. Overlapped I/O operation is in progress

安装 VS2017 碰到 Error 997。Solution #1 + #4 混搭，搞定。

### Solution #1 - Change the name of the Microsoft\Crypto\RSA folder

Rename:

```
C:\ProgramData\Microsoft\Crypto\RSA\S-1-5-18 
to
C:\ProgramData\Microsoft\Crypto\RSA\S-1-5-18_BAK
```

### Solution #2 - Apply the following hotfix

Download and install [this Microsoft hotfix][3] to correct errors introduced by Microsoft security update KB2918614.

### Solution #3 - Remove the updates triggering the errors

 * Go to Control Panel > Uninstall a program (or Programs and Features).
 * In the left menu, click View installed updates.
 * In the search box at the top-right, search for KB2918614.
 * If you find it, uninstall it.
 * Do another search for KB3072630, KB3000988, and KB3008627.
 * If you find them, uninstall them as well.
 * Restart your computer.

### Solution #4 - Edit the Registry

 * HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Installer
 * **SecureRepairPolicy**, DWORD, value = 1

### Reference

 * [autodesk article][1]
 * [stackoverflow article][2]


[1]:https://knowledge.autodesk.com/search-result/caas/sfdcarticles/sfdcarticles/Install-Failure-Error-997-Overlapped-I-O-operation-is-in-progress.html
[2]:https://stackoverflow.com/questions/39081136/vc-installs-error-1603-error-997-overlapped-i-o-operation-is-in-progress
[3]:https://support.microsoft.com/en-us/help/3000988/-the-profile-for-the-user-is-a-temporary-profile-error-when-you-instal
[4]:https://www.youtube.com/watch?v=SnRyP-QYs20
[5]:https://www.minitool.com/backup-tips/irql-not-less-or-equal-windows-10-021.html
[6]:https://www.thewindowsclub.com/windows-memory-diagnostics-tool-in-windows-7
