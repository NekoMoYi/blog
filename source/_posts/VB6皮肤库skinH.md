---
title: VB6皮肤库skinH
date: 2020-07-05 14:26:43
tags: VB
categories: [代码, 后端]
---
见惯了拥有新设计语言的程序，VB的官方控件真的有些看不下去，就加了个SkinH_VB6.dll来常常换下皮肤让自己看得舒服点awa
<!--more-->
### 下载链接
[点击下载: skinH.zip](https://wwi.lanzous.com/iA07ljyh30d)
### 使用方法
解压skinH_VB6.dll和skin文件夹，放在vb工程根目录下。
在代码中加入如下几行即可使用皮肤。
```vb
'Skin_H
Private Declare Function SkinH_AttachEx Lib "SkinH_VB6.dll" (ByVal lpSkinFile As String, ByVal lpPasswd As String) As Long
    
Private Sub Form_Load()
        SkinH_AttachEx App.Path & "/skin/skin.she" , ""
End Sub
```
### 效果展示
![Office2007皮肤](https://7.dusays.com/2021/01/02/071400f9d45a9.png)
此处使用的是office2007的皮肤，虽然有点老，但胜在更换方便。看腻了就换一个呗233