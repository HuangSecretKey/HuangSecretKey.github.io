---
title: matlab总结
date: 2020-12-15 21:04:48
tags: -matlab
---

平常科研当中，当我们在看文献时，每看到一个优秀的算法时都有想要自己动手编程去实现的愿望，算法好坏可以用代码的运行时间来评估，在MATLAB中大致有以下几种方法来计算程序的运行时间：

<!-- more -->

### 1、tic和toc组合

```
tic
%代码块
toc
%disp(['运行时间: ',num2str(toc)]);1234
```

程序遇到tic时Matlab自动开始计时，运行到toc时自动计算此时与最近一次tic之间的时间

### 2、etime()与clock组合

```
t1=clock;
%代码块
t2=clock;
etime(t2,t1)1234
```

计算t1，t2之间的时间差，它是通过调用windows系统的时钟进行时间差计算得到运行时间的。

### 3、cputime函数

```
t0=cputime
%代码块
t1=cputime-t0123
```

上以上三种方法，都是可以进行程序运行时间的计算，但是Matlab官方推荐使用tic，toc组合。但是使用tic/toc的时候一定要注意，toc计算的是与最后一次（即离它最近）运行的tic之间的时间。

参考：http://www.matlabsky.com/thread-2607-1-1.html?_dsign=33d7d995