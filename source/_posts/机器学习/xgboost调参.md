---
title: XGBoost调参
date: 2018-06-20 19:28:44 
tags: XGBoost
categories: 机器学习
---



- 选择较高的**学习速率(learning rate)**。一般情况下，学习速率的值为0.1。但是，对于不同的问题，理想的学习速率有时候会在0.05到0.3之间波动。选择**对应于此学习速率的理想决策树数量**。XGBoost有一个很有用的函数“cv”，这个函数可以在每一次迭代中使用交叉验证，并返回理想的决策树数量。

- 对于给定的学习速率和决策树数量，进行**决策树特定参数调优**(max_depth, min_child_weight, gamma, subsample, colsample_bytree)。在确定一棵树的过程中，我们可以选择不同的参数，待会儿我会举例说明。

- xgboost的**正则化参数**的调优。(lambda, alpha)。这些参数可以降低模型的复杂度，从而提高模型的表现。

- 降低学习速率，确定理想参数。

 

xgboost第n颗树的第n层的分裂准则：

![image-20200605153827635](http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2023-09-14-125857.jpg)
