---
title: 理解召回和LTR
date: 2022-08-10 
categories: 搜推广
---

参考文章：[万变不离其宗：用统一框架理解向量化召回](https://mp.weixin.qq.com/s/E5a4TF9P2aMrF6gVatAF8A)

# 召回本质

召回本质上和排序其实存在很大的区别：

- 召回模型特征是解耦的，排序模型的特征是交叉的，这个是性能上的考虑。
- 召回是样本的艺术，排序是特征的艺术。召回的负样本如果用的是曝光未点击，和排序类似的话，就有问题，这样召回的训练和预估的数据分布不一致了。因此召回的负样本是随机采样。



# 召回的几种类型

- i2i:拿用户喜欢的item找相似item。就比如work2vec这种，或者图构建item embeding这种，都是拿用户的点击序列去构建，然后计算item emb，然后找到用户喜欢的item的相似的emb。
- u2i：直接给user找他可能喜欢的item。这种是目前应用最多的，例如基于DNN的FM双塔模型，就是用户一个emb，item一个emb，最后内积计算得分。
- u2u2i:给user查找相似user，再把相似user喜欢的item推出去。



# 召回的负采样

召回负采样的做法不仅仅只是随机采样，一般会有类似work2vec的做法，对热门item正样本降权，负样本加权这种操作。

负采样也分为easy negaitve和hard negative两种：

- easy negaitve ：随机采样一般就是这种
- Hard negaive ：比如正样本是狗，猫、大象明显是easy negative，而狼就和狗有几分相似，但又不是的称为hard negative。一般做法有基于业务逻辑自己加，还有FB的做法：上一个召回模型预估得分的中间位置作为下一个召回模型的hard negative。



# LTR

Learning to Rank其实是针对排序算法的统称，针对loss function，可分为三种：

### Pointwise

最为常见，像精排这种明确的正负样本，直接binary cross entropy计算label 准确度。缺点是没有关注到排序信息。



### Pairwise

更关系两个样本的排序顺序，一般像召回任务很多都是用pairwise，常见的几种loss有：



#### sampled softmax loss

![image-20210806211523927](https://tva1.sinaimg.cn/large/e6c9d24ely1h51rlpiypvj21c60jcq7e.jpg)

这个就像word2vec里的计算的sampled softmax loss，值得注意的是，这个loss做梯度更新的方式，每一个样本喂进去之后都需要做负采样(会有个辅助emb矩阵，得到负样本分母的emb)，然后计算loss，然后更新梯度，不像常见的交叉熵的梯度更新。



#### margin hinge loss

![image-20210806211842642](https://tva1.sinaimg.cn/large/e6c9d24ely1h51rm81ox5j219u056754.jpg)

#### BPR Loss

![image-20210806211857913](https://tva1.sinaimg.cn/large/e6c9d24ely1h51rmbwy31j21co09cdhj.jpg)

对于后面两种loss，样本都需要是三元形式，《u , item+,item-》，后面两个item都要过一遍nn得到emb。

