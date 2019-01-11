专注APT攻击与防御
https://micropoor.blogspot.com/

从xp开始默认有.net framework,在powershell后，调用起来更方便。

连载1
System.Data.SqlClient 命名空间是用于 SQL Server 的 .NET 数据提供程序。在net framework2.0中新增加SqlDataSourceEnumerator类。提供了一种枚举本地网络内的所有可用SQL Server实例机制。微软官方是这样解释的：

>   SQL Server 2000 和 SQL Server 2005 进行应用程序可以确定在当前网络中的 SQL Server实例存在。SqlDataSourceEnumerator类公开给应用程序开发人员，提供此信息DataTable包含所有可用的服务器的信息。返回此表列出了与列表匹配提供当用户尝试创建新的连接的服务器实例以及Connection Properties对话框中，展开下拉列表，其中包含所有可用的服务器。

```
PowerShell -Command 
"[System.Data.Sql.SqlDataSourceEnumerator]::Instance.GetDataSources()"
```
![](media/56279a8ac03519b94850df0f82c9835a.jpg)

![](media/cfcaa6274999ba01237a3d0265ed7b45.jpg)

此种方法，在实战中，不留文件痕迹。并且信息准确，发现主机也可。可应对目前主流安全防御产品。

>   Micropoor
