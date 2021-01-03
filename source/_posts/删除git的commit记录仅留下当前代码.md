---
title: 删除git的commit记录仅留下当前代码
date: 2020-07-05 14:14:53
tags: Git
categories: Git
---
有时候git仓库乱七八糟的东西加了又删太多会导致无法push，因此需要清空一下commit记录
<!--more-->
```shell
#检出版本
git checkout --orphan latest_branch
#添加文件
git add -A
#提交
git commit -am "cleaning"
#删除master分支
git branch -D master
#重命名当前分支为master
git branch -m master
#推送
git push -f origin master
```