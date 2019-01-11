专注APT攻击与防御
https://micropoor.blogspot.com/

**ARP简介：**
ARP,通过解析网路层地址来找寻数据链路层地址的一个在网络协议包中极其重要的网络传输协议。根据IP地址获取物理地址的一个TCP/IP协议。主机发送信息时将包含目标IP地址的ARP请求广播到网络上的所有主机，并接收返回消息，以此确定目标的物理地址

**1.nmap扫描**

`root@John:~# nmap -sn -PR 192.168.1.1/24`
![](media/5dfd9a546cd24575a6f3dcc700e27fdb.jpg)

**2.msf扫描**
```
msf > use auxiliary/scanner/discovery/arp_sweep 
msf auxiliary(arp_sweep) > show options

Module options (auxiliary/scanner/discovery/arp_sweep):

Name Current Setting Required Description
---- --------------- -------- -----------
INTERFACE no The name of the interface
RHOSTS yes The target address range or CIDR identifier
SHOST no Source IP Address
SMAC no Source MAC Address
THREADS 1 yes The number of concurrent threads
TIMEOUT 5 yes The number of seconds to wait for new data

msf auxiliary(arp_sweep) > set RHOSTS 192.168.1.0/24 
RHOSTS => 192.168.1.0/24
msf auxiliary(arp_sweep) > set THREADS 10
```
![](media/185d0b136875716cb9602245c9c83dc1.jpg)
![](media/61293e9ad861848052a95221f7da4b21.jpg)

**3.netdiscover**

`root@John:~# netdiscover -r 192.168.1.0/24 -i wlan0`
![](media/d521395b5907857c22c4e677dcfc0181.jpg)
![](media/e014ab5145b99e17c902cec765d171ce.jpg)

**4.arp-scan（linux）**
(推荐)速度与快捷
项目地址：https://linux.die.net/man/1/arp-scan
arp-scan没有内置kali，需要下载安装。
![](media/1f572b4553deb15c1d8c84a16ba53142.jpg)
![](media/c3c2dffb726780a766b4ef3cd573ced0.jpg)

**5.Powershell**

`c:\tmp>powershell.exe -exec bypass -Command "Import-Module .\arpscan.ps1;Invoke-ARPScan -CIDR 192.168.1.0/24"`

![](media/97ef86dfca58678be449d5b8e5ed6aaf.jpg)

**6.arp scannet**
项目地址：
https://sourceforge.net/projects/arpscannet/files/arpscannet/arpscannet%200.4/
![](media/727cc7e2717a361bf93d66bdb922b5d6.jpg)

**7.arp-scan（windows）**

(推荐)速度与快捷
arp-scan.exe -t 192.168.1.1/24
项目地址：https://github.com/QbsuranAlang/arp-scan-windows-/tree/master/arp-scan
（非官方）

![](media/92ce763215c297168c4514992758d91b.jpg)

**8.arp-ping.exe**
arp-ping.exe 192.168.1.100
![](media/302ee6c2b41a771ab5df7c337de15d8e.jpg)

**9.其他**
如cain的arp发现，一些开源py，pl脚本等，不一一介绍。

**附录：**
以上非内置文件网盘位置。**后门自查**。
链接：https://pan.baidu.com/s/1boYuraJ 
密码：58wf

>   Micropoor
