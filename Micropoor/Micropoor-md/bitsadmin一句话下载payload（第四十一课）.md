专注APT攻击与防御
https://micropoor.blogspot.com/

**微软官方做出如下解释：**
BITSAdmin是一个命令行工具，可用于创建下载或上传并监视其进度。

**具体相关参数参见官方文档：**
https://docs.microsoft.com/zh-cn/windows/desktop/Bits/bitsadmin-tool

自windows7以上版本内置bitsadmin，它可以在网络不稳定的状态下下载文件，出错会自动重试，在比较复杂的网络环境下，有着不错的性能。

**靶机：windows 7**

`E:\>bitsadmin /rawreturn /transfer down "http://192.168.1.115/robots.txt" E:\PDF\robots.txt`
![](media/68d44cdc3d8dfab1799b7becdd141031.jpg)

需要注意的是，bitsadmin要求服务器支持Range标头。

如果需要下载过大的文件，需要提高优先级。配合上面的下载命令。再次执行

`bitsadmin /setpriority down foreground`

如果下载文件在1-5M之间，需要时时查看进度。同样它也支持进度条。

`bitsadmin /transfer down /download /priority normal "http://192.168.1.115/robots.txt" E:\PDF\robots.txt`

![](media/362901f0c27b936f6065b7b91b567395.jpg)


>   后者的话：不支持https协议。
>   Micropoor
