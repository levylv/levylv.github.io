---
title: Uplift model理解
date: 2022-10-25
categories: 搜推广
---



# 背景

在一些文档中看到了uplift模型，此类模型在一些用户增长(营销发券)，甚至广告模型中都有用到，因此做了一些查阅，发现确实是个蛮有意思的方向，从另一个切入点来建模广告营销场景，整理成blog记录下。

[参考文献](https://www.6aiq.com/article/1585121131929)



# 建模

<img src="https://tva1.sinaimg.cn/large/008vxvgGgy1h7hom6obdxj30mg0gydh0.jpg" alt="image-20221025174957436" style="zoom: 67%;" />

以cvr模型为例，传统的cvr模型预估的是假设给用户曝光广告后的转化率，而没有考虑假设不曝光广告的自然转化率，uplift建模的则是该差值，并且期望找到的用户群体为：未曝光则不转化，曝光后则转化的这类群体。



# 算法

uplift算法有多种：

- two model: 本质上还是两个cvr模型，一个是有曝光样本，一个是自然样本，建模两个模型，然后差值算uplift
- one model: 本质还是建模cvr，只是将曝光与否作为特征送入模型
- 基于树模型的uplift model: 直接建模uplift，方法比较巧妙，借鉴了决策树的思想，期望让分裂的左右子树的uplift差异尽可能大。

评估：auuc方法
