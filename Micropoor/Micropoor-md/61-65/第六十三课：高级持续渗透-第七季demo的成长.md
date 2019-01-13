专注APT攻击与防御
https://micropoor.blogspot.com/

本季是作《PHP安全新闻早八点-高级持续渗透-第六季关于后门》的补充。
* https://micropoor.blogspot.com/2018/12/php.html
* **原本以为第六季的demo便结束了notepad++**
* **但是demo系列的懿旨并没有按照作者的想法来表述。顾引入第七季。**

在第一季关于后门中，文章提到重新编译notepad++，来引入有目标源码后门构造。

在第六季关于后门中，文章**假设在不得知notepad++的源码**，来引入无目标源码沟门构造。

而第七季关于后门中，让这个demo更贴合于实战。此季让这个demo成长起来。它的
成长痕迹分别为第一季，第六季，第七季。

**该系列仅做后门思路。**

**懿旨：安全是一个链安全，攻击引入链攻击，后门引入链后门。让渗透变得更加有趣。**

**Demo 环境：**
* Windows 2003 x64
* Windows 7 x64
* notepad++ 7.6.1，notepad++7.5.9 
* vs 2017

**靶机以notepad++ 7.5.9为例：**
默认安装notepad++流程图，如下：一路下一步。
![](media/b8aac18b874785f0c8bc969f75b77a65.jpg)

![](media/2f08b914ac8b52592b2b7e0993f62040.jpg)

![](media/3794646c3d867b1761b4195a01ee0735.jpg)

**目标机背景：** windows 2003，x64，notepad++ 7.6.1，notepad++7.5.9，iis，aspx

![](media/c20afc4dccc75cb4a95cb2e46575b4fd.jpg)

**shell权限如下：**
![](media/2b489f025ac208347291fd42ed7a6a1a.jpg)

**notepad++7.5.9**
* 安装路径：E:\Notepad++\
* 插件路径：E:\Notepad++\plugins\

![](media/6837e346988cc844adf8ed91f6c40103.jpg)

**检查默认安装情况如下：**
![](media/3fc723c290e8134bb050a2d6ff692351.jpg)

注：为了让本季的demo可观性，顾不打算隐藏自身。
![](media/0f916c3a715bd62f5f92ff97ebe29572.jpg)

端口如下：
![](media/4bbdb3e33cf96125bdf0886ac59eaefc.jpg)

shell下写入：
**注：**
**notepad++ v7.6以下版本插件路径为：**  
`X:\Notepad++\plugins\`

**notepad++ v7.6以上版本插件路径为：**
`X:\Documents and Settings\All Users\Application Data\Notepad++\plugins`

![](media/1cbf6f87cfa8d6d24fdb4f755c5210dc.jpg)

目标机管理员再次打开notepad++：
**注：demo中不隐藏自身**
![](media/10feb51a838194fbcc604cb596b1c9b3.jpg)

端口变化如下：
![](media/f96508fcfb44dd1cff346ccdaad50fdb.jpg)

msf 连接目标机：
![](media/637712a99f114ff8c074927dc8f3a756.jpg)

**后者的话：**

如果此demo，增加隐身自身，并demo功能为：增加隐藏帐号呢？或者往指定邮箱发目标机帐号密码明文呢？如果当第六季依然无法把该demo加入到实战中，那么请回顾。这样实战变得更为有趣。**安全是一个链安全，攻击引入链攻击，后门引入链后门。让渗透变得更加有趣。**

>   Micropoor
