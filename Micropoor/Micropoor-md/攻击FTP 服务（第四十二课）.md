专注APT攻击与防御
https://micropoor.blogspot.com/

在办公区的内网中，充斥着大量的ftp文件服务器。其中不乏有部分敏感文件，也许有你需要的密码文件，也许有任务中的目标文件等。本季从讲述内网ftp服务器的发现以及常用的相关模块。

**靶机介绍：**
* 靶机一：Windows 2003 | 192.168.1.115
* 靶机二：Debian | 192.168.1.5

msf内置search模块，在实战中，为了更快速的找到对应模块，它提供了type参数（未来会具体讲到模块参数），以ftp模块为例。
```
msf > search type:auxiliary ftp

Matching Modules
 ================

Name Disclosure Date Rank Description
---- --------------- ---- -----------
auxiliary/admin/cisco/vpn_3000_ftp_bypass 2006-08-23 normal Cisco VPN Concentrator 3000 FTP Unauthorized Administrative Access
auxiliary/admin/officescan/tmlisten_traversal normal TrendMicro OfficeScanNT Listener Traversal Arbitrary File Access
auxiliary/admin/tftp/tftp_transfer_util normal TFTP File Transfer Utility
auxiliary/dos/scada/d20_tftp_overflow 2012-01-19 normal General Electric D20ME TFTP Server Buffer Overflow DoS
auxiliary/dos/windows/ftp/filezilla_admin_user 2005-11-07 normal FileZilla FTP Server Admin Interface Denial of Service
......
```
![](media/9f4d6fb344a6812126526c03dc4cdb2e.jpg)

**auxiliary/scanner/ftp/ftp_version**
![](media/afc593576266cfa54f2dcd0eeefcfa81.jpg)

**auxiliary/scanner/ftp/ftp_login**
![](media/6d172564dfc97a31f39366bedca0baf9.jpg)

**auxiliary/scanner/ftp/anonymous**
![](media/956d2d88181d499ab543583448d60aa9.jpg)

当然msf也内置了nmap，来内网大量发现FTP存活主机，参数与使用与nmap一致。
`msf auxiliary(scanner/ftp/anonymous) > db_nmap -sS -T4 -p21 192.168.1.115`
![](media/79c9ed79b71ccc537313c6b1bbd2d477.jpg)

msf更多针对了ftpd。
![](media/f96780563285d4c96f6acefa51214740.jpg)

**ftp本地模糊测试辅助模块：**
![](media/7cf2c4df632790edc7b4af6042f58388.jpg)

**auxiliary/fuzzers/ftp/ftp_pre_post**
![](media/20cdea228c480bc89f22f69987d877df.jpg)

关于ftp的f本地uzzer，更推荐的是本地fuzz，msf做辅助poc。
![](media/d19bd4335259bd091a4ab4e9e6ba3fe5.jpg)

![](media/06293311884d5469f50199e889ede3f6.jpg)

关于后期利用，poc编写，在未来的季中会继续讲述。

>   Micropoor
