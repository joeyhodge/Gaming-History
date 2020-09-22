# Error 997. Overlapped I/O operation is in progress

安装 VS2017 碰到 Error 997。Solution #1 + #4 混搭，搞定。


## Solution #1 - Change the name of the Microsoft\Crypto\RSA folder

Rename:

```
C:\ProgramData\Microsoft\Crypto\RSA\S-1-5-18 
to
C:\ProgramData\Microsoft\Crypto\RSA\S-1-5-18_BAK
```


## Solution #2 - Apply the following hotfix

Download and install [this Microsoft hotfix][3] to correct errors introduced by Microsoft security update KB2918614.


## Solution #3 - Remove the updates triggering the errors

 * Go to Control Panel > Uninstall a program (or Programs and Features).
 * In the left menu, click View installed updates.
 * In the search box at the top-right, search for KB2918614.
 * If you find it, uninstall it.
 * Do another search for KB3072630, KB3000988, and KB3008627.
 * If you find them, uninstall them as well.
 * Restart your computer.


## Solution #4 - Edit the Registry

 * HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Installer
 * **SecureRepairPolicy**, DWORD, value = 1


## Reference

 * [autodesk article][1]
 * [stackoverflow article][2]

[1]:https://knowledge.autodesk.com/search-result/caas/sfdcarticles/sfdcarticles/Install-Failure-Error-997-Overlapped-I-O-operation-is-in-progress.html
[2]:https://stackoverflow.com/questions/39081136/vc-installs-error-1603-error-997-overlapped-i-o-operation-is-in-progress
[3]:https://support.microsoft.com/en-us/help/3000988/-the-profile-for-the-user-is-a-temporary-profile-error-when-you-instal
