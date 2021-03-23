---
title: Anaconda清华源镜像的使用及pytorch安装总结
date: 2021-01-24 20:07:31
tags: -pytorch
categories: -深度学习tricks
---
每次重新安装pytorch时都经常翻看教程，故在此记录
<!--more-->

pytorch 各版本安装的链接
```
https://pytorch.org/get-started/previous-versions/
```
## Anaoconda
在安装好anaconda后，添加清华镜像源，故在安装其他软件及软件包时自动检测从清华镜像源安装，速度快。
### 添加清华镜像源至Anaconda仓库中
运行下列命令，将清华镜像源添加至Anaconda仓库中。
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```
### Conda Forge
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
```
### msys2
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
```
### biocloud
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
```
### menpo
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
```
### pytorch
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
conda config --add channels https://mirrors.tuna.tsinfhua.edu.cn/anaconda/cloud/peterjc123/
```

## 安装pytorch
找到相应的命令后，输入到终端中安装
<p align="center">
<a>
  <img src="images/pytorch版本安装命令.png" alt="pytorch版本安装命令" >
</a >
</p >

!!! 注意，要将最后的-c pytorch去掉