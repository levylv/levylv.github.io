---
title: Tensorflow一些常用方法记录
date: 2020-10-28
tags: Tensorflow
categories: ML框架
---



### tf.stack

tf.stack(values, axis=0, name="stack"):

```python
  x = tf.constant([1, 4])
  y = tf.constant([2, 5])
  z = tf.constant([3, 6])
  tf.stack([x, y, z])  # [[1, 4], [2, 5], [3, 6]] (Pack along first dim.)
  tf.stack([x, y, z], axis=1)  # [[1, 2, 3], [4, 5, 6]]
```

本质是把一个长度为N的list，list里的元素有自己的shape：

- axis = 0，则变为（N，shape[0], shape[1], ...）
- axis = 1，则变为（shape[0], N, shape[1], ...）

常见用法有：

如果元素为长度为M的list

- Axis =0,  其实就相当于横着concate
- Axis =1，相当于把元素看为列向量，然后竖着concate



### tf.gather和tf.gather_nd

tf.gather(params, indices, validate_indices=None, name=None, axis=0):

对于params,就是你的输入，axis就是你进行操作的维度，默认为0(对于tf.gather,只能对于给定的axis操作),indices就是你想获取的axis维度上的值，来看一个例子，可以更佳清楚的了解函数是如何使用的：

```python
temp = tf.range(0,10)*10 + tf.constant(1,shape=[10])

temp2 = tf.gather(temp,[1,5,9])

with tf.Session() as sess:

print sess.run(temp)

print sess.run(temp2)

输出

[ 1 11 21 31 41 51 61 71 81 91]

[11 51 91]
```

tf.gather只能对低维度进行操作，gather_nd可以进行高维度的操作。和gather相比，没有了axis参数，如果indices的维度和params的维度等价，他是直接获取一个具体值(之后组合成向量)。



### tf.expand_dims和tf.squeeze

- tf.expand_dims(input, axis=None)，就是在对应的axis上加个1的维度，这样可以让tensorflow的元素乘和元素加运算可以用上broadcast机制。
- broadcast机制：满足下面两个条件即可进行元素复制。
  - 从后往前维度相同
  - 遇到不同的话，必须有一个维度为1.
  - 举例：
    - a.shape = (4,1,3,3), b.shape(1,3), a+b = (4,1,3,3)
    - a.shape = (4,1,1,3), b.shape(3,1), a+b = (4,1,3,3)

- tf.squeeze则相反，去掉维度为1的维度。



### tf.split()

将矩阵的某一维度切分为一个list。

- 'value' is a tensor with shape [5, 30]
- split0, split1, split2 = tf.split(value, [4, 15, 11], 1)
- tf.shape(split0)  # [5, 4]
- tf.shape(split1)  # [5, 15]
- tf.shape(split2)  # [5, 11]



### tf.add_n()

将input list里的tensor按元素相加，和普通相加没什么区别，只是可以加个operation name