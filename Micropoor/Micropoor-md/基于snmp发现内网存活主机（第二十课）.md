专注APT攻击与防御
https://micropoor.blogspot.com/

**SNMP简介：**
SNMP是一种简单网络管理协议，它属于TCP/IP五层协议中的应用层协议，用于网络管理的协议。SNMP主要用于网络设备的管理。SNMP协议主要由两大部分构成：SNMP管理站和SNMP代理。SNMP管理站是一个中心节点，负责收集维护各个SNMP元素的信息，并对这些信息进行处理，最后反馈给网络管理员；而SNMP代理是运行在各个被管理的网络节点之上，负责统计该节点的各项信息，并且负责与SNMP管理站交互，接收并执行管理站的命令，上传各种本地的网络信息。

**nmap扫描：**

`root@John:~# nmap -sU --script snmp-brute 192.168.1.0/24 -T4`
![](media/045b3f2f2c7da50d9f2b8a1ad9f86a0e.jpg)

**msf扫描：**
`msf > use auxiliary/scanner/snmp/snmp_enum`
![](media/4f92b1b39415a27bc404e01c05ef6bfb.jpg)

项目地址：https://www.mcafee.com/us/downloads/free-tools/snscan.aspx依然是一块macafee出品的攻击
![](media/b8c3f5e36ed6a251a9d7c6ff3f00f665.jpg)

**NetCrunch：**

项目地址：https://www.adremsoft.com/demo/
内网安全审计工具，包含了DNS审计，ping扫描，端口，网络服务等。
![](media/19aed1cd1327808c60a4f944fe83a838.jpg)


**snmp for pl扫描：**

项目地址：https://github.com/dheiland-r7/snmp
![](media/f0ab120e48d6c92d5c5596352f81355d.jpg)

![](media/0f3812a0ec50201e000ae56a7d127210.jpg)


**其他扫描：**
snmpbulkwalk：
![](media/6d6d4d08c98c2861e776b8f01b977bb7.jpg)

snmp-check：
![](media/54949180a485e0f4ec2884da361a2bce.jpg)

snmptest：
![](media/d34215b3490fae6d1722a950facdf77e.jpg)

**附录：**

```
use auxiliary/scanner/snmp/aix_version use auxiliary/scanner/snmp/snmp_enum
use auxiliary/scanner/snmp/arris_dg950
use auxiliary/scanner/snmp/snmp_enum_hp_laserjet
use auxiliary/scanner/snmp/brocade_enumhash use auxiliary/scanner/snmp/snmp_enumshares 
use auxiliary/scanner/snmp/cambium_snmp_loot use auxiliary/scanner/snmp/snmp_enumusers
use auxiliary/scanner/snmp/cisco_config_tftp use auxiliary/scanner/snmp/snmp_login
use auxiliary/scanner/snmp/cisco_upload_file use auxiliary/scanner/snmp/snmp_set
use auxiliary/scanner/snmp/netopia_enum
use auxiliary/scanner/snmp/ubee_ddw3611 
use auxiliary/scanner/snmp/sbg6580_enum
use auxiliary/scanner/snmp/xerox_workcentre_enumusers
```

其他内网安全审计工具（snmp）：
项目地址：https://www.solarwinds.com/topics/snmp-scanner
项目地址：https://www.netscantools.com/nstpro_snmp.html

**snmp for pl ：**
Can't locate NetAddr/IP
![](media/bc665f1b06550f7d8e81ec4ef5dfbbb8.jpg)

`root@John:~/Desktop/snmp# wget http://www.cpan.org/modules/by-module/NetAddr/NetAddr-IP-4.078.tar.gz`
![](media/ce0a8f4a495c9a33f152c1d4e996a021.jpg)

`root@John:~/Desktop/snmp# tar xvzf ./NetAddr-IP-4.078.tar.gz`
![](media/cd8ede5c8e7edeaa7e7c7901c0a7c4b2.jpg)

```
root@John:~/Desktop/snmp# cd NetAddr-IP-4.078/
root@John:~/Desktop/snmp/NetAddr-IP-4.078# ls
About-NetAddr-IP.txt Artistic Changes 
Copying docs IP.pm Lite Makefile.PL 
MANIFEST MANIFEST.SKIP META.yml t TODO
root@John:~/Desktop/snmp/NetAddr-IP-4.078# perl Makefile.PL
```
![](media/b095c67e64484673aca21d97843a8275.jpg)

`root@John:~/Desktop/snmp/NetAddr-IP-4.078# make`
![](media/42807ab04c2def49a5d85520601c49ca.jpg)

`root@John:~/Desktop/snmp/NetAddr-IP-4.078# make install`
![](media/7c5dfdd85bc7f5f74a30cb799e92beb1.jpg)

\> \_ \< !!
![](media/f171e221b9c374de99d4f973a5b5b400.jpg)

>   Micropoor
