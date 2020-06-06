---
title: Tensorflow2.0的几个变动
date: 2019-11-18 22:06:02
tags: Tensorflow
categories: ML框架
---



## 1. 原来API的变动

原先v1.0的诸多API都是发生了变动或者删除：

- 删除了tf.variable_scope, tf.get_variable

- tf.layer弃用，高层api统一用tf.keras

- 原先一些api也都不要用了，目前可以用tf.compat.v1.*来代替，但是只维护一年。

  
## 2. 动态图机制

- Eager execution作为默认工作模式
    - Eager Execution（动态图机制）是TensorFlow的一个命令式编程环境，它无需构建计算图，可以直接评估你的操作：直接返回具体值，而不是构建完计算图后再返回。
    - 有了Eager Execution，我们不再需要事先定义计算图，然后再在session里评估它。它允许用python语句控制模型的结构。
- AutoGraph
    - 在 TensorFlow 1.x 版本中，要开发基于张量控制流的程序，必须使用 tf.conf、tf. while_loop 之类的专用函数。这增加了开发的复杂度。
    - 在 TensorFlow 2.x 版本中，可以通过自动图（AutoGraph）功能，将普通的 Python 控制流语句转成基于张量的运算图，大大简化了开发工作。

总体来看，动态图的机制和pytorch极像。目前为了稳妥起见，打算先不升级到2.0，用tf.13就行，高层api统一切换到tf.keras

