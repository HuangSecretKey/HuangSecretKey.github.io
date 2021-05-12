---
title: PCA数学原理解析
date: 2021-04-21 19:20:17
tags: [总结;原理;tricks]
categories: [教程]
---

PCA降维度原理，原文来自于https://wenku.baidu.com/view/8fdc079ac850ad02df804148.html

PCA本质上是将方差最大的方向作为主要特征，并且在各个正交方向上将数据“离相关”，也就是让它们在不同正交方向上没有相关性。

PCA缺陷：

PCA也存在一些限制。例如它可以很好的解除线性相关，但是对于高阶相关性就没有办法了。
PCA假设数据各主特征是分布在正交方向上，如果在非正交方向上存在几个方差较大的方向，PCA的效果就大打折扣了。

<!--more-->

<img src="assets/1619004148686.png" alt="1619004148686" style="zoom: 67%;" />

#### 向量表示

<img src="assets/1619004224618.png" alt="1619004224618" style="zoom:67%;" />

#### 目的和原则

<img src="assets/1619004247011.png" alt="1619004247011" style="zoom:67%;" />

#### 降维与相关性举例

<img src="assets/1619004261149.png" alt="1619004261149" style="zoom:67%;" />

<img src="assets/1619004279074.png" alt="1619004279074" style="zoom:67%;" />![1619004313836](assets/1619004313836.png)

![1619004313836](assets/1619004313836.png)

<img src="assets/1619004326411.png" alt="1619004326411" style="zoom:67%;" />

#### 向量的表示及基变换

<img src="assets/1619004345037.png" alt="1619004345037" style="zoom:67%;" />

<img src="assets/1619004369495.png" alt="1619004369495" style="zoom:67%;" />

<img src="assets/1619004394591.png" alt="1619004394591" style="zoom:67%;" />

##### 基变换的矩阵表示

<img src="assets/1619004418301.png" alt="1619004418301" style="zoom:67%;" />

#### 协方差矩阵及优化目标

<img src="assets/1619004437633.png" alt="1619004437633" style="zoom:67%;" />

##### 方差

<img src="assets/1619004451622.png" alt="1619004451622" style="zoom:67%;" />

##### 协方差

<img src="assets/1619004473610.png" alt="1619004473610" style="zoom:67%;" />

##### 协方差矩阵及推广

<img src="assets/1619004487585.png" alt="1619004487585" style="zoom:67%;" />

<img src="assets/1619004502453.png" alt="1619004502453" style="zoom:67%;" />

<img src="assets/1619004516080.png" alt="1619004516080" style="zoom:67%;" />

<img src="assets/1619004530220.png" alt="1619004530220" style="zoom:67%;" />



PCA算法

<img src="assets/1619004542900.png" alt="1619004542900" style="zoom:67%;" />

<img src="assets/1619004558159.png" alt="1619004558159" style="zoom:67%;" />

<img src="assets/1619004568958.png" alt="1619004568958" style="zoom:67%;" />