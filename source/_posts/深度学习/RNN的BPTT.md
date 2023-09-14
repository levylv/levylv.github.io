---
title: RNN的BPTT
date: 2021-01-08
categories: 深度学习
---



RNN采用的梯度更新策略是BPTT，梯度分为两部分：

- 竖直方向的输出层权重：这个和传统反向传播没什么区别，因为这一层的权重只与当前loss有关
- 竖直方向的输入层和水平权重：这个就稍微复杂点，因为rnn的总loss是所有时刻loss相加的，同时每个时刻的loss又会影响到这里所说的所有权重，所以操作是：
  - 计算某个时刻Et的误差项，通过反向传播来计算，最后计算梯度。
  - 汇总所有的时刻计算的梯度。

<img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2023-09-14-130143.jpg" alt="image-20210108172855248" style="zoom:50%;" />

[参考资料](https://www.cnblogs.com/wacc/p/5341670.html)

