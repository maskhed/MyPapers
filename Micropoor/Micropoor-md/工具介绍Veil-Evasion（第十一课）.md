专注APT攻击与防御
https://micropoor.blogspot.com/

项目地址：
https://github.com/Veil-Framework/Veil-Evasion

Veil-Evasion是与Metasploit生成相兼容的Payload的一款辅助框架，并可以绕过大多数的杀软。

Veil-Evasion并没有集成在kali，配置sources.list，可直接apt-get。
```bash
root@John:~/Deskto#cat /etc/apt/sources.list

#中科大
deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
#阿里云
#deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
#deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
#清华大学
#deb http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free 
#deb-src https://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free 
#浙大
#deb http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free
#deb-src http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free
#东软大学
#deb http://mirrors.neusoft.edu.cn/kali kali-rolling/main non-free contrib
#deb-src http://mirrors.neusoft.edu.cn/kali kali-rolling/main non-free contrib 
#官方源
deb http://http.kali.org/kali kali-rolling main non-free contrib 
deb-src http://http.kali.org/kali kali-rolling main non-free contrib 
#重庆大学
#deb http://http.kali.org/kali kali-rolling main non-free contrib 
#deb-src http://http.kali.org/kali kali-rolling main non-free contrib
```
![](media/0bf8f9c39799eabc1ad3525e6635d060.jpg)

`root@John:~/Desktop# apt-get install veil-evasion`

由于在实验中本机已经安装，所以我们在虚拟机中使用git方式来下载和安装。（以便截图）ps:本次kali下截图使用scrot
```
root@John:~/Deskto# apt-get install scrot
root@John:~/Deskto# scrot -s //即可
root@John:~/Deskto# git clone https://github.com/Veil-Framework/Veil-Evasion.git
```
![](media/9665385060c79f6175e74032ad2e5f5f.jpg)

`root@John:~/Veil-Evasion# ./setup.sh //安装漫长`
![](media/b1a88b6e639e83ba04dcb815f1be3035.jpg)

![](media/65807c037d2e0d7f6330de1be8bf7801.jpg)

![](media/a6dd85c8811146b386d748891dc40ee8.jpg)

以c/meterpreter/rev_tcp为例：
![](media/6d0b6469b0b5b2cfcee03a1d9544441a.jpg)

![](media/10b9071c854c0a2eeea642cc6fd4023c.jpg)

ps:Veil-Evasion不在更新，新版本项目地址：
https://github.com/Veil-Framework/Veil

**附录：**
[*] 可支持生成payloads:
1) auxiliary/coldwar_wrapper
2) auxiliary/macro_converter
3) auxiliary/pyinstaller_wrapper
4) c/meterpreter/rev_http
5) c/meterpreter/rev_http_service
6) c/meterpreter/rev_tcp
7) c/meterpreter/rev_tcp_service
8) c/shellcode_inject/flatc
9) cs/meterpreter/rev_http
10) cs/meterpreter/rev_https
11) cs/meterpreter/rev_tcp
12) cs/shellcode_inject/base64_substitution
13) cs/shellcode_inject/virtual
14) go/meterpreter/rev_http
15) go/meterpreter/rev_https
16) go/meterpreter/rev_tcp
17) go/shellcode_inject/virtual
18) native/backdoor_factory
19) native/hyperion
20) native/pe_scrambler
21) perl/shellcode_inject/flat
22) powershell/meterpreter/rev_http
23) powershell/meterpreter/rev_https
24) powershell/meterpreter/rev_tcp
25) powershell/shellcode_inject/download_virtual
26) powershell/shellcode_inject/download_virtual_https
27) powershell/shellcode_inject/psexec_virtual
28) powershell/shellcode_inject/virtual
29) python/meterpreter/bind_tcp
30) python/meterpreter/rev_http
31) python/meterpreter/rev_http_contained
32) python/meterpreter/rev_https
33) python/meterpreter/rev_https_contained
34) python/meterpreter/rev_tcp
35) python/shellcode_inject/aes_encrypt
36) python/shellcode_inject/aes_encrypt_HTTPKEY_Request
37) python/shellcode_inject/arc_encrypt
38) python/shellcode_inject/base64_substitution
39) python/shellcode_inject/des_encrypt
40) python/shellcode_inject/download_inject
41) python/shellcode_inject/flat
42) python/shellcode_inject/letter_substitution
43) python/shellcode_inject/pidinject
44) python/shellcode_inject/stallion
45) ruby/meterpreter/rev_http
46) ruby/meterpreter/rev_http_contained
47) ruby/meterpreter/rev_https
48) ruby/meterpreter/rev_https_contained
49) ruby/meterpreter/rev_tcp
50) ruby/shellcode_inject/base64
51) ruby/shellcode_inject/flat

>   Micropoor
