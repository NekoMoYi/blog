---
title: UWP应用无法联网以及代理设置
date: 2020-04-15 10:17:45
tags: [UWP,无法联网,SSR,代理,已解决]
categories: [遇到的问题,windows,网络]
copyright: true
---

某日照例打开小破站的第三方UWP想要寻找快乐，结果却蹦出来个网络连接错误
<!--more-->
<img src="https://image.moyi.ml/2020/04/15/bilibili.png" alt="bilibili.png" style="zoom: 67%;" />

因为这个UWP不是官方所作，而且之前也遇到过这样的问题，但是过了一天就好了，因此我以为是接口没有更新什么的，就没怎么注意，打开官网直接看网页版了。

但是晚些时候想研究RSS订阅，看到有推荐微软商店里的NewsFlow，就准备去下载个一探究竟，结果又是无法连接的错误...

<img src="https://image.moyi.ml/2020/04/15/msstore.png" alt="msstore.png" style="zoom: 67%;" />

然后我又天真的以为是微软商店的锅（毕竟微软的网络服务经常不稳定嘛）

但是当我打开QQ音乐UWP准备放首歌听听：

<img src="https://image.moyi.ml/2020/04/15/qqmusic.png" alt="qqmusic.png" style="zoom:67%;" />

诶诶，这怕我网络是出问题了

于是我又打开了网易云音乐的UWP版本

<img src="https://image.moyi.ml/2020/04/15/neteasemusic.png" alt="neteasemusic.png" style="zoom:67%;" />

行了，这下确定是我的问题了QwQ

但是浏览器、QQ什么的一点问题都没有啊。于是只能求助于baidu

铺天盖地的全都是说要勾选IPV6、更改网络属性为公用、重置网络连接。但是我全部都试了一遍，完全不起作用啊

于是又准备求助于404小厂，结果发现：代理服务器错误。在向系统托盘一看——woc，我的小飞机呢？崩了！

行了，这下破案了。因为懒得去设置浏览器代理规则，所以小飞机直接改的是系统代理的PAC模式

![proxy.png](https://image.moyi.ml/2020/04/15/proxy.png)

结果谁知道小飞机崩了呢，走系统代理的应用就连接不上代理服务器了

但是为什么浏览器啥的却又都能联网呢？

搜索了一下发现很多人都有UWP应用无法走代理的情况，原因是

[UWP应用是类似于沙盒应用，无法使用系统的代理，如果想要用google登陆UWP应用，会由于无法使用系统搭理而代理登陆失败。]: https://www.jianshu.com/p/106fe1421b83

但是非UWP应用走的本地连接，却是能正常使用的。



以及如何让UWP应用通过代理访问网络的解决方法也在上面这篇文章找到了答案：

```vbscript
    Set ws = WScript.CreateObject("wscript.shell")  
    app = ws.ExpandEnvironmentStrings("%USERPROFILE%\AppData\Local\Packages")
    Set fso = WScript.CreateObject("scripting.filesystemobject")
    Set fs = fso.GetFolder(app).SubFolders
    Set bat = fso.createtextfile(ws.ExpandEnvironmentStrings("%TEMP%\WindowsAppProxyAccess.bat"))  
    For Each f In fs  
        bat.WriteLine ("CheckNetIsolation.exe LoopbackExempt -a -n=" & f.name)  
    Next  
    bat.WriteLine ("del %0")  
    CreateObject("Shell.Application").ShellExecute ws.ExpandEnvironmentStrings("%TEMP%\WindowsAppProxyAccess.bat"),"","","runas",1   
```

运行此VBS脚本即可解除UWP的代理限制。