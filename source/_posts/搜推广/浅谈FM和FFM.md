---
title: 浅谈FM和FFM
date: 2022-08-10
categories: 搜推广
---

### 梳理一下FM和FFM

#### FM：

- 参数复杂度：O(n*k)

- 计算复杂度：O(n^2 * k)，但可以通过目标函数转换变为O(n * k)


<img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2023-09-14-130727.jpg" alt="image-20201020184158031" style="zoom:40%;" />


#### FFM:

<img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2023-09-14-130729.jpg" alt="image-20201020184225396" style="zoom:33%;" />

- 参数复杂度：O(nkF)

- 计算复杂度：O(n^2 * k)，无法简化。**注意：每个特征都有自己所在的域，特征1和特征2交叉时，特征1的域向量选择特征2所在域F2，特征2的域向量选择特征1所在域F1**。其实FFM就是把一个one-hot 变量中FM对应的向量，变成一个矩阵。

  

FM与DeepFM的对应：

- DeepFM的FM层，就是one-hot 到 embedding层，这个本质上就是对每个one-hot变量都做一个隐向量。因为一个one-hot同时只有一个维度为1，其余为0，所以同一个field之间没有内积，做field之间的内积本质上就FM。

FFM与DeepFFM的对应：

- 无非就是embeding层从一个向量，变成了一个矩阵，m * F（域的个数）。两个域内积的时候注意是对应的。

  <img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2023-09-14-130733.jpg" alt="image-20201224111301818" style="zoom:50%;" />



#### 总结

传统的FFM公式，通常用神经网络来代替，通常做法就是将n个特征划分成若干个field，每个域做一个embedding，得到每个域的embedding向量和bias向量(相当于公式的一次项部分)。

- Embedding层参数量：n*k，bias参数量：n

紧接着有很多处理方式：

- 如果是低阶特征抽取，那么就是两个域之间做inner product，得到二次项交叉特征。例如DeepFFM的wide层
- 如果是高阶特征抽取，那么可以concat这些embedding层(维度为f*k)，后面接mlp层。例如DeepFFM的deep层
- bias的话一般用于wide - LR层 or FM层



### Ref-深度学习在CTR预估中的应用

https://zhuanlan.zhihu.com/p/35484389

![image-20200930165622880](http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2023-09-14-130722.jpg)



<img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2023-09-14-130737.jpg" alt="image-20200930165727535" style="zoom:50%;" />

