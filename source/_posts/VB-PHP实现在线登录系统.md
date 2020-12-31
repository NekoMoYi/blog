---
title: VB+PHP实现在线登录系统
date: 2020-07-05 14:38:37
tags: [VB, PHP]
categories: [VB, PHP]
---
## 起因
信息课上老师一直只允许写VB的代码，于是准备用VB整点好活:-P。前几天找了些资料，熟悉了一下各种函数，以PHP为服务器脚本，VB为客户端程序做出来一个登录界面。
<!--more-->
## 内容
### 客户端
#### UI设计
![登录界面UI](http://image.moyi.ml/2020/07/05/loginform.png)
#### 函数代码
```vb
'Skin_H
Private Declare Function SkinH_AttachEx Lib "SkinH_VB6.dll" (ByVal lpSkinFile As String, ByVal lpPasswd As String) As Long
'INI
Option Explicit
Private Declare Function GetPrivateProfileString Lib "Kernel32" Alias "GetPrivateProfileStringA" _
(ByVal lpApplicationName As String, lpKeyName As Any, ByVal lpDefault As String, _
ByVal lpRetunedString As String, ByVal nSize As Long, ByVal lpFileName As String) As Long
 
'读取INI
Public Function GetINI(AppName As String, KeyName As String, FileName As String) As String
   Dim retstr As String
   retstr = String(255, chr(0))
   GetINI = Left(retstr, GetPrivateProfileString(AppName, ByVal KeyName, "", _
              retstr, Len(retstr), FileName))
End Function
'切分字符串函数
Public Function SplitStr(str As String, chr As String, num As Integer)
    Dim i, flag, count As Integer, recent As String
    SplitStr = "": flag = 0: count = 1
    For i = 1 To Len(str)
        recent = Mid(str, i, 1)
        If recent <> chr And count = num Then
                SplitStr = SplitStr + Mid(str, i, 1)
            ElseIf recent = chr Then
                count = count + 1
        End If
    Next i
End Function
'网络请求
Public Function getHTML(strUrl As String)
    On Error Resume Next
    Dim xmlhttp As Object, stime, ntime
    Set xmlhttp = CreateObject("Microsoft.XMLHTTP")
    xmlhttp.open "POST", strUrl, True
    xmlhttp.send
    stime = Now
    While xmlhttp.ReadyState <> 4
    DoEvents
        ntime = Now
        If DateDiff("s", stime, ntime) > 3 Then
            getHTML = ""
            Exit Function
        End If
    Wend
    getHTML = StrConv(xmlhttp.responseBody, vbUnicode)
    Set xmlhttp = Nothing
End Function

'点击登录
Private Sub CLogin_Click()
    Dim url, args, retstr As String
    If TUID.Text = "" Or TPassword.Text = "" Then MsgBox ("不可为空"): Exit Sub
    If Len(TPassword.Text) < 6 Then
        MsgBox ("密码太短")
        Exit Sub
    End If
    CLogin.Enabled = False
    url = "http://" & GetINI("config", "server", App.Path & "/config.ini") & "/login"
    args = "?uid=" & TUID.Text & "&password=" & TPassword.Text
    retstr = getHTML(url + args)
    '奇怪的返回判断增加了！
    If SplitStr(retstr, "|", 1) = "SUCCESS" Then
        MainForm.Show '显示主界面
        FLogin.Hide
    ElseIf SplitStr(retstr, "|", 1) = "FAILED" Then
        MsgBox ("失败: " + SplitStr(retstr, "|", 2))
        TPassword.Text = ""
    Else
        MsgBox ("网络错误")
    End If
    CLogin.Enabled = True
End Sub

Private Sub Form_Load()
    SkinH_AttachEx App.Path & "/skin/" & GetINI("config", "skin", App.Path & "/config.ini"), "" '加载皮肤
    LVersion.Caption = "v" & GetINI("config", "version", App.Path & "/config.ini")
End Sub

Private Sub Form_Unload(Cancel As Integer)
    End '结束程序
End Sub
```
#### config.ini

```ini
[config]
version=0.0
server=localhost
skin=qqgame.she
```



#### 效果图

![登录界面](http://image.moyi.ml/2020/07/05/loginform1.png)

### 服务端
服务端只是用来检测客户端代码可用性的(lll￢ω￢)就随便瞎写了点
#### /login/index.php
```PHP
<?php
//没有任何技术含量的页面
if($_REQUEST['uid']=="admin" && $_REQUEST['password']=="123456"){
    echo "SUCCESS|testToken";
    session_start();
    $_SESSION['name'] = "admin";
}
else{echo "FAILED|Something wrong";}
?>
```

## 完成

这样一个简单的登录就成了，后续就把它应用在别的工程上吧o(*￣▽￣*)ブ