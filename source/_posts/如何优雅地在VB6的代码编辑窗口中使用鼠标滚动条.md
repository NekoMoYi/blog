---
title: 如何优雅地在VB6的代码编辑窗口中使用鼠标滚动条
date: 2020-07-05 13:51:23
tags: VB
categories: VB
---
虽然很久之前就有说信息教材要改版成python，但是这一届的教材并没有更新，而是等到下一届。所以我们信息技术高考居然还是要考VB...
<!--more-->
一开始因为偏见比较排斥VB，没了`{}`和`;`总觉得哪里不对，但是没过多久也就习惯了。真正上手之后才发现其实还是有挺多好处的。比如在学校的老爷机上编译速度可比新的VS快上太多了哇！就不用再享受代码改一行、编译几分钟的快乐了（淦）



但是话说回来，毕竟还是二十多年前的IDE了，总归有一些兼容性bug。比如在代码编辑窗口居然不能用鼠标滚轮滚动？？？

网上找了找发现都有出现这个bug，接着翻到了微软官方的补丁。

[点击下载: VB6IDEMouseWheelAddin.zip](http://storage.moyi.ml/2020/07/05/VB6IDEMouseWheelAddin.zip)

解压后放到C:\windows\system32里面，接着以管理员模式运行cmd，输入命令

```powershell
regsvr32 VB6IDEMouseWheelAddin.dll
```

重启VB之后即可愉快地使用滚动条辣！