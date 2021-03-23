---
title: github网站打不开解决方案
date: 2021-03-12 15:29:14
tags: [-bug, -github, -website]
categories: -bug总结
---
github网站从放假回来后一直登陆不上，开始以为是vpn设置的原因，因为网络代理导致无法登陆，在修改vpn节点和更改电脑静态ip上一直不起作用。找了很多方法，要求在c:\windows\systems32\drivers\etc\host中添加许多ip相关的语句，但不知道其含义，也不起作用。
<!--more-->
后来搜索发现了解决方法：
### 1.查看Github的ip地址
    访问http://www.github.com.ipaddresss.com/ 查看ip地址，我的是140.82.113.4

### 在hosts文件最后将ip地址添加进去即可
    140.82.113.4   github.com
    windows下hosts文件的路径为: C:\Windows\System32\drivers\etc\
### 如果在保存hosts文件时没有权限
    需要按照下面这篇文章的步骤获取修改hosts文件的权限
    https://jingyan.baidu.com/article/624e7459b194f134e8ba5a8e.html
    