专注APT攻击与防御
https://micropoor.blogspot.com/

在写第五季的时候，vps掉线了，ssh重新登录后，无法切到MSF session下，想到部分同学如果在vps上操作也会遇到这个问题，故本季解决该问题。

**tmux是什么？**

Tmux是一个优秀的终端复用软件，类似GNU Screen，但来自于OpenBSD，采用BSD授权。使用它最直观的好处就是，通过一个终端登录远程主机并运行tmux后，在其中可以开启多个控制台而无需再“浪费”多余的终端来连接这台远程主机。是BSD实现的Screen替代品，相对于Screen，它更加先进：支持屏幕切分，而且具备丰富的命令行参数，使其可以灵活、动态的进行各种布局和操作。

**Tmux的使用场景**
1. 可以某个程序在执行时一直是输出状态，需要结合nohup、&来放在后台执行，并且ctrl+c结束。这时可以打开一个Tmux窗口，在该窗口里执行这个程序，用来保证该程序一直在执行中，只要Tmux这个窗口不关闭
2. 公司需要备份数据库时，数据量巨大，备份两三天弄不完，这时不小心关闭了终端窗口或误操作就前功尽弃了，使用Tmux会话运行命令或任务，就不用担心这些问题。
3. 下班后，你需要断开ssh或关闭电脑，将运行的命令或任务放置后台运行。
4. 关闭终端,再次打开时原终端里面的任务进程依然不会中断
5. 在渗透过程中，意外因网络等原因ssh掉线，tmux可以恢复session会话

![](media/d47fa61934f99368662fb6fb6dd39d84.jpg)

![](media/1b621f6005d83138d3a71662a08ac9d0.jpg)

![](media/49159b718def6a0fe947df35f156c680.jpg)

**tmux 常用操作命令：**
* tmux new -s session1 新建会话
* ctrl+b d 退出会话，回到shell的终端环境 //tmux detach-client 
* tmux ls 终端环境查看会话列表
* ctrl+b s 会话环境查看会话列表
* tmux a -t session1 从终端环境进入会话
* tmux kill-session -t session1 销毁会话
* tmux rename -t old_session_name new_session_name 重命名会话
* ctrl + b $ 重命名会话 (在会话环境中)

还原会话
![](media/c3741ac0522e6ae0e9c0e232caf06aef.jpg)

>   Micropoor
