---
title: 关于stop_gradient和trainable的理解
date: 2023-07-10
tags: Tensorflow
categories: ML框架
---



两个方法的分别意义：

- stop_gradient参考[资料](https://zhuanlan.zhihu.com/p/35003434)

- 而trainable则是在初始化变量时，手动设置为True or Flase，意图为是否加入到trainable_variable中(计算梯度时的变量集合)。



两个方法都可以起到停止梯度更新的作用，但stop_gradient更加灵活一点，可以直接截断路径上的前面所有变量的梯度。
