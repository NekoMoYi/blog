---
title: 个人主页和博客转移到gitee以及遇到的一些坑
date: 2020-09-26 10:35:30
tags:
---
近来不知道发生了什么，coding pages的访问是越来越慢了，甚至到后来完全都无法连接。
本来就是冲着访问速度去的，不然早用github pages了。现在速度比gayhub pages还慢那是完全忍受不了了。遂转移到gitee
虽然不能自动编译部署了，但好在访问是真滴流畅。
从coding克隆了仓库编译commit push一气呵成，更新部署
随后打开，发现MDI图标一个都出不来。。
<!--more-->
打开console发现数条类似如下的记录
```
Access to font at 'https://moyiljx.gitee.io/static/fonts/materialdesignicons-webfont.51c686b.ttf' from origin 'http://moyiljx.gitee.io' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```
字体文件因为跨域的问题不能加载，但是直接点击控制台出现的链接却又一切正常。
搜索了相关错误找到的都是在Nginx配置文件里把Access-Control-Allow-Origin里面字体文件略过
但这是码云的服务器啊！明显不行好嘛╮(╯-╰)╭
打开gitee pages服务页面，发现只有三个选项
```
部署分支
部署目录
强制使用HTTPS
```
上面两个明显没有关系，那就打开HTTPS试一下好了。
嗯
好了
凸(艹皿艹 )

接下来是博客的问题
因为blog放在另外的仓库，访问链接后面要加/blog，所以一开始只能加载出文本，并且各类链接都有乱七八糟的问题
修改_config.yml
```yml
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://moyiljx.gitee.io/blog
root: /blog
```
大部分资源都可以访问了，但是ayer主题配置的搜索js还是无法加载
时间紧张 先不管了
能用就行
淦