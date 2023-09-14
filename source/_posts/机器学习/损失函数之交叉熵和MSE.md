---
title: 损失函数之交叉熵和MSE
date: 2019-11-18 19:28:44 
categories: 机器学习
---



一般神经网络都是用交叉熵，回归问题用MSE，分类问题用交叉熵。

网友分析了一下，发现MSE对梯度有个不友好的地方，有时候误差越大，梯度越小，而交叉熵是正比的。

![image-20200605153953308](http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2023-09-14-125901.jpg)

MSE : $A^2 * (1-A)$

cross entropy : A

 

另外，一般推动神经网络BP算法的时候，用的都是MSE，如果用cross entry和softmax的话，会比较复杂一点。

 
