---
title: python常用函数总结
date: 2021-01-12 15:28:16
tags: [-python, -pytorch]
categories: -函数总结
---

用于总结代码学习中的函数等，持续更新中...
<!--more-->

## permute函数的用法
### permute(dims)

将tensor的维度调换位置
参数： -_dims_(int..*)- 换位顺序

```
x = torch.randn(2,3,5)
x.size()
torch.Size([2,3,5])
x.permute(2,0,1).size()
torch.Size([5,2,3])
```
### permute与transpose函数的区别
Tensor.permute(a,b,c,d..): permute函数可以对任意高维矩阵进行维度变换，但是没有torch.permute()这种调用方式，只有Tensor.permute()：
```
torch.randn(2,3,4,5).permute(3,2,0,1).shape
torch.Size([5,4,2,3])
```
#### transpose函数
torch.transpose(Tensor, a, b):transpose只能操作2D矩阵的转置，有两种调用方式，上述两种方式都可以使用
```
torch.randn(2,3,4,5).transpose(3,0).transpose(2,1).transpose(3,2).shape
torch.Size([5, 4, 2, 3])
torch.radn(2,3,4,5).transpose(1,0).transpose(2,1).transpose(3,1).shape
torch.Size([3, 5, 2, 4])
```

### permute 与contiguous、view函数关联

#### contiguous 
contiguous:view只能作用于contiguous的varible上，如果在view之前调用了transpose、permute等，需要调用函数contiguous()返回一个contiguous copy。

一种可能的解释：有些tensor并不是在内存中不是连续的，是由不同的数据块组成的，而tensor的view()操作依赖于内存是整块的，这是需要用contiguous将tensor()变成在内存中连续分布的形式。
判断tensor是否为contiguous，可以调用torch.Tensor.is_contiguous()函数
```
import torch
x = torch.ones(10,10)
x.is_contiguous() # True
x.transpose(0, 1).is_contiguous() #false
x.transpose(0, 1).contiguous().is_contiguous() # true
```
注:在pytorch4.0版本中，增加了torch.reshape()，与numpy.reshape()的功能类似，大致相当于tensor.contiguous().view()，这样就省去了对tensor做view()变换前调用contiguous()的麻烦。
#### permute与view函数的功能展示
```
import torch
import numpy as np
a = np.array([[[1,2,3],[4,5,6]]])
unpermuted = torch.tensor(a)
print(unpermuted.size()) # torch.Size([1,2,3])

permuted = unpermuted.permute(2,0,1)
print(permuted.size()) # torch.size([3,1,2])

view_test = unpermuted.view(1,3,2)
print(view_test.size()) 
```
利用permute(2,0,1)将Tensor([[[1,2,3],[4,5,6]]])转换成：
```
tensor([[[1,4]],[[2,5]],[[3,6]]])
```
而使用view(1,3,2)可以得到
```
tensor([[[1,2],
        [3,4],
        [5,6]]])
```