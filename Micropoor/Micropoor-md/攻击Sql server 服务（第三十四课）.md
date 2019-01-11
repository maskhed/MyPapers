专注APT攻击与防御
https://micropoor.blogspot.com/

msf 内置关于mssql插件如下（部分非测试mssql 插件）
![](media/14d4b2ef52542a9214714790bb6814bd.jpg)

关于msf常用攻击mssql插件如下：
1. auxiliary/admin/mssql/mssql_enum
2. auxiliary/admin/mssql/mssql_enum_sql_logins
3. auxiliary/admin/mssql/mssql_escalate_dbowner
4. auxiliary/admin/mssql/mssql_exec
5. auxiliary/admin/mssql/mssql_sql
6. auxiliary/admin/mssql/mssql_sql_file
7. auxiliary/scanner/mssql/mssql_hashdump
8. auxiliary/scanner/mssql/mssql_login
9. auxiliary/scanner/mssql/mssql_ping
10. exploit/windows/mssql/mssql_payload
11. post/windows/manage/mssql_local_auth_bypass

本地靶机测试：x86 windows 2003 ip:192.168.1.115
![](media/53d6d9288fecdd2e130300f85c918fde.jpg)

1. auxiliary/admin/mssql/mssql_enum

非常详细的目标机Sql server 信息：
![](media/811bc0093d66031f0e18375fa737a07c.jpg)
![](media/311969e0450cb1458a954f92b02d2088.jpg)

2.auxiliary/admin/mssql/mssql_enum_sql_logins

枚举sql logins，速度较慢，不建议使用。
![](media/c2f4ae385eeee038db62565323c03853.jpg)

3.auxiliary/admin/mssql/mssql_escalate_dbowner

发现dbowner，当sa无法得知密码的时候，或者需要其他账号提供来支撑下一步的内网渗透。
![](media/7edd4d6780a48d93d787f59296c85928.jpg)

4.auxiliary/admin/mssql/mssql_exec

最常用模块之一，当没有激活xp_cmdshell，自动激活。并且调用执行cmd命令。权限继承 Sql server。
![](media/3759eb9026a6b73dcd9e3a3a582cbf8b.jpg)

5.auxiliary/admin/mssql/mssql_sql

最常用模块之一，如果熟悉Sql server 数据库特性，以及sql语句。建议该模块，更稳定。

![](media/a8d69a38790844c0b37057687853064e.jpg)

6.auxiliary/admin/mssql/mssql_sql_file

当需要执行多条sql语句的时候，或者非常复杂。msf本身支持执行sql文件。授权渗透应用较少，非授权应用较多的模块。
![](media/8cf7347d1403235e63419c9c4e461c1e.jpg)

7.auxiliary/scanner/mssql/mssql_hashdump
mssql的hash导出。如果熟悉sql语句。也可以用mssql_sql模块来执行。如图2.

![](media/1ac1d8066a613adab1f09e0d6d707ab6.jpg)
图2：
![](media/e2c5ee21db0a1a811c9333331312abb7.jpg)

8.auxiliary/scanner/mssql/mssql_login

内网渗透中的常用模块之一，支持RHOSTS，来批量发现内网mssql主机。mssql的特性除了此种方法。还有其他方法来专门针对mssql主机发现，以后得季会提到。
![](media/6e7aaf7b86627694ee979784cc8aaa7c.jpg)

9.auxiliary/scanner/mssql/mssql_ping
查询mssql 实例，实战中，应用较少。信息可能不准确。
![](media/cf3c52e619f19e45e2266648279a0f0d.jpg)

10.exploit/windows/mssql/mssql_payload

非常好的模块之一，在实战中。针对不同时间版本的系统都有着自己独特的方式来上传payload。
![](media/f38228156f2a8c7285c2bb48b6105ef0.jpg)

**注：由于本季的靶机是 windows 2003，故参数set method old，如果本次的参数为cmd，那么payload将会失败。**

![](media/5826f503abb5f2108d0d392b3c6d9f6d.jpg)

11.post/windows/manage/mssql_local_auth_bypass

post模块都属于后渗透模块，不属于本季内容。未来的系列。会主讲post类模块。

>   后者的话：
在内网横向渗透中，需要大量的主机发现来保证渗透的过程。而以上的插件，在内网横向或者Sql server主机发现的过程中，尤为重要。与Mysql 不同的是，在Sql server的模块中，一定要注意参数的配备以及payload的组合。否则无法反弹payload。

>   Micropoor
