专注APT攻击与防御
https://micropoor.blogspot.com/

**任务背景：**

需要得知周某某的今年采购的其中一个项目具体信息，目前已知该成员是xxx电网。负责丰满大坝的采购人员。整体思路如下：

找到开发公司，得到源码，审计问题，得到shell，拿到服务器，得到域控（或者终端管理）。得到个人机。下载任务文件。

得知该电网公司电网相关网站是某公司出品，得到某公司对外宣传网站，并且得到该公司服务器权限，下载源码模板。

**源码审计：**

全局共计2个主要文件，分别是Func on.asp，Startup.asp

后台验证项：Function.asp
来源验证：
![](media/f3d00f2e404aed2ace94202fad9196e1.jpg)

注入验证：
（目标服务器waf，遂放弃）
![](media/69876bba47950df97d93d92afc6db16e.jpg)

错误处理：
![](media/84612708e4d4f301fd655d48dc05267a.jpg)

XSS字符处理：
![](media/2e3dd4d6f449ed860b790f4022b17291.jpg)

直接输入admin/下文件名处理：
![](media/99f0ee3d3f7cd1ff3965502aff91bf1e.jpg)

目录生成：针对iis6以及iis7 php版本
![](media/0a28fcfc4ba6e48eb857cc6b18e374e5.jpg)

Startup.asp
配置文件：当不可以执行的时候，是否可以备份出数据库，以便下载。
![](media/47c48653bf1053dce5cee81320444f63.jpg)

关于新闻显示，全局incude head.asp
![](media/2275d60bafee51dd0adf82a42b9d079b.jpg)

其中check_si.asp 主要为防止注入
Get注入
![](media/398190858612948b7bee70a83bd145c2.jpg)

Post 注入 新版本中加入post注入

过程中遇到服务器卡顿现象，也就是不清楚列名数，本地二分法测试如下：
![](media/6c8bfb4c6de07bb3a93296288a15bbb0.jpg)

![](media/664d42e0571762dcbe9c25f6b43928b1.jpg)

![](media/f6c06ab3aefa6fffd987c9c7c90ed3e0.jpg)

在admin 目录下有个database.asp文件
![](media/8271f2240cf178ee4a6fba221aeaedc2.jpg)

**目标测试：**

根据以上信息，构造referrer，构造参数，禁止js。产生出越权漏洞。
![](media/5d66957a8586118f1ca10d23a0815a85.jpg)

![](media/bff69d4a2c2a89ef8703ec99fd8fc8e3.jpg)

根据越权漏洞，继续看upload.asp文件，允许匿名上传图片文件。在根据越权漏洞备份出webshell文件
![](media/35aca0797d0299bb8c104d8be4bb3d6c.jpg)
![](media/cc1c8766b6439d3e3c011ae8250066ad.jpg)

82：
![](media/18f0b435a0ce1ccbb750a46e4b8df563.jpg)
![](media/f1e074099cd43a5aa7057f57762b768f.jpg)

得到webshell
![](media/17ab0b85f64e92e48eaeeb32ef0f83aa.jpg)

对方没有开启远程桌面：
开启：
`REG ADD HKLM\SYSTEM\CurrentControlSet\Control\Terminal" "Server /v fDenyTSConnections /t REG_DWORD /d 00000000 /f`

通过该服务器得到mssql 数据库。得到终端管理权限。
![](media/ae7a57c36b08aeb2176945f9e2072e6d.jpg)

![](media/ccc30aaffee18324e722073588b55c9b.jpg)

查看在线机器，查找目标人物。
![](media/7e76f878e0182df67f910a01ccfa315c.jpg)

![](media/9906ffaba02adcbfef821163286b9104.jpg)

推送payload 反弹。
![](media/09d81485119e132856f82757b8418f69.jpg)
![](media/9938674149690d748853a0adf86e8fe3.jpg)

确定是否为目标人物：采购员 桌面截图
![](media/545e99d1852fd486c825fc29598b0a0d.jpg)
![](media/04deeea27d64d51d2a0826d5d66b488d.jpg)

按照任务 取得该人员的其中一个xls文件
![](media/f67aea48578dce29a104af040d4cbe74.jpg)
![](media/ec0b56268a8689d959c65ca976e3cd90.jpg)

任务完成。
![](media/a90bcbaa1e1bb96809cfb8afc4a4dc22.jpg)



>   Micropoor
