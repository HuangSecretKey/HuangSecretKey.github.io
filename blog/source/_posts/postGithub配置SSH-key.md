---
title: github 配置SSH key
date: 2020-08-31 19:40:46
tags: -github
---

Github配置SSH Key

在本地生成ssh 密钥

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
然后会在本机的.ssh文件夹下产生密钥文件，ubuntu在home文件夹下，ctrl + H 显示隐藏文件可找到，生成过程如下：
```
Generating public/private rsa key pair
```
回车选择保存路径
```
Enter a file in which to save the key(/Users/you/.ssh/id_rsa):[Press enter]
```
然后输入两遍密码，这里密码是用来加密密钥的，以后使用密钥的时候需要输入此密码，可以不输入，一直回车。
```
Enter passphrase(empty for no passphrase):[Type a passphrase]
Enter same passphrase again:[Type passphrase again]
```
打开SSH密钥所在的.ssh文件夹(windows可能会找不到，可以自行到上边提到的目录下寻找)
```
cd ./.ssh
```
打开id_rsa.pub文件，复制ssh-rsa开头的内容
登录[Github](github.com),进入setting,ssh and gpg key，设置title，把复制的密钥粘贴到下面。
<p align="center">
  <a>
    <img src="images/ssh key.png" alt="harware-deployment" width="800" height="400">
  </a >
  <h3 align="center"></h3>  
</p >

测试ssh是否添加成功

```{.line-numbers}
$ ssh -T git@github.com
The authenticity of host 'github.com (192.30.252.1)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
The authenticity of host 'github.com (192.30.252.1)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)?
Enter passphrase for key '/User/yourmacusername/.ssh/id_rsa':
Hi username! You've successfully authenticated, but GitHub does not
provide shell access.
```