# 专注APT攻击与防御
https://micropoor.blogspot.com/

**ICMP简介：**
它是TCP/IP协议族的一个子协议，用于在IP主机、路由器之间传递控制消息。控制消息是指网络通不通、主机是否可达、路由是否可用等网络本身的消息。这些控制消息虽然并不传输用户数据，但是对于用户数据的传递起着重要的作用。

**nmap扫描：**
```bash
root@John:~# nmap ‐sP ‐PI 192.168.1.0/24 ‐T4
```
![](media/32074ff9a8a71e3f75239e68c5161b17.jpg)

```bash
root@John:~# nmap ‐sn ‐PE ‐T4 192.168.1.0/24
```
![](media/0f2404547901a8c72cc03544c5961259.jpg)

**CMD下扫描：**
```bash
for /L %P in (1,1,254) DO @ping ‐w 1 ‐n 1 192.168.1.%P | findstr "TTL ="
```
![](media/ab265501d9c11081cb0f63e3cb991d80.jpg)

**powershell扫描：**
```powershell
powershell.exe ‐exec bypass ‐Command "Import‐Module ./Invoke‐TSPingSweep.ps1
; Invoke‐TSPingSweep ‐StartAddress 192.168.1.1 ‐EndAddress 192.168.1.254 ‐Resolv
eHost ‐ScanPort ‐Port 445,135"
```
![](media/7feea7aca005154fdbef4180ed5a9aae.jpg)

![](media/9c8dbccee70c90adc40f48e69c473df8.jpg)

```bash
D:\>tcping.exe ‐n 1 192.168.1.0 80
```
![](media/351e700a9da6780fb709932a7b0b56f7.jpg)

**附录:**
powershell脚本与tcping（来源互联网，后门自查）
链接：https://pan.baidu.com/s/1dEWUBNN 密码：9vge

>   Micropoor
