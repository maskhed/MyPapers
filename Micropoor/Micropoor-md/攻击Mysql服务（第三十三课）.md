专注APT攻击与防御
https://micropoor.blogspot.com/

msf 内置关于mysql插件如下（部分非测试mysql 插件）
![](media/876ad30bffd3f57b961ecb2781f888fc.jpg)

关于msf常用攻击mysql插件如下：
1. auxiliary/scanner/mysql/mysql_login
2. exploit/multi/mysql/mysql_udf_payload
3. exploit/windows/mysql/mysql_mof
4. exploit/windows/mysql/scrutinizer_upload_exec
5. auxiliary/scanner/mysql/mysql_hashdump
6. auxiliary/admin/mysql/mysql_sql
7. auxiliary/scanner/mysql/mysql_version

以下本地靶机测试：
靶机1：x86 Windows7
![](media/ff8b69c7beb814d667497bbc433cfaaa.jpg)

靶机2 ：x86 windows 2003 ip:192.168.1.115
![](media/5692738d486e9997fe0752e7e6be1c9f.jpg)


**1.auxiliary/scanner/mysql/mysql_login**

常用于内网中的批量以及单主机的登录测试。
![](media/c9f16b174fa0a7b4f67c75b3ed9f2492.jpg)

**2.exploit/multi/mysql/mysql_udf_payload**

常用于root启动的mysql 并root的udf提权。
![](media/9b06e9685123fd026cb108f9d281c05f.jpg)
![](media/8569841b55b9fd3bf0ed467eb4f9daf3.jpg)

![](media/0a4ac046fdc3e4d1f7d713ef14f1651c.jpg)

**3.exploit/windows/mysql/mysql_mof**

以上类似，提权。
![](media/cc563fb9d228e4c7adaf7b284925dbf4.jpg)

**4.exploit/windows/mysql/scrutinizer_upload_exec**

上传文件执行。

![](media/904116076e8cc90398e6769b4a8f0492.jpg)

**5.auxiliary/scanner/mysql/mysql_hashdump**

mysql的mysql.user表的hash
![](media/0ccc9d7da3cbd464b35662a431824eea.jpg)

而在实战中，mysql_hashdump这个插件相对其他较为少用。一般情况建议使用sql语句：
更直观，更定制化

![](media/8e1d6085009d34a2afbbab9f4bddd95f.jpg)


**6.auxiliary/admin/mysql/mysql_sql**

执行sql语句。尤其是在目标机没有web界面等无法用脚本执行的环境。
![](media/42b5a61ec763f6c6dbd725650188e5ad.jpg)

![](media/f1fb35b04225b40e571c5e61f8516886.jpg)

**7.auxiliary/scanner/mysql/mysql_version**

常用于内网中的批量mysql主机发现。

![](media/e9bfe12061674c64cdee00c7056173f1.jpg)

>   后者的话：
在内网横向渗透中，需要大量的主机发现来保证渗透的过程。而以上的插件，在内网横向或者mysql主机发现的过程中，尤为重要。

>   Micropoor
