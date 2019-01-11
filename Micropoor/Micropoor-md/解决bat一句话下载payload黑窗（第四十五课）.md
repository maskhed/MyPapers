专注APT攻击与防御
https://micropoor.blogspot.com/

实战中，需要用bat解决的事情总会碰到，而针对不同的环境，可能同一件事情需要不同的方案。

**demo:测试bat**
bat 内容：追加到bat.txt里。
![](media/c88eaa0827ffcda7b8bf145ab166128e.jpg)

![](media/ac6b7ca7ff474a41f6c4e3b39740c63e.jpg)

附代码：
```
Set oShell = CreateObject ("Wscript.Shell")
Dim strArgs
strArgs = "cmd /c bat.bat"
oShell.Run strArgs, 0, false
```

但是代码过长，需要追加写入。需要简化下代码。
![](media/4459b76b982a0d54de0cba76496b2f07.jpg)

附代码：
`CreateObject("Wscript.Shell").Run "bat.bat", 0, True`

如果需要在目标机上执行多个bat，如果需要把代码中的bat.bat变成变量的话。
![](media/362d2fd19ae38ea13dd9f7f4b7e7b845.jpg)

**附代码：**
```
If WScript.Arguments.Count \>= 1 Then
ReDim arr(WScript.Arguments.Count‐1)
For i = 0 To WScript.Arguments.Count‐1
Arg = WScript.Arguments(i)
If InStr(Arg, " ") > 0 Then Arg = """" & Arg & """"
arr(i) = Arg
Next 

RunCmd = Join(arr)
CreateObject("Wscript.Shell").Run RunCmd, 0, True
End If
```

>   Micropoor
