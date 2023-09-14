---
title: XGBoost和LightGBM调参
date: 2018-12-05 19:28:44 
tags: XGBoost
categories: 机器学习
---


有个应用上的重大区别：

- Xgboost只处理数值特征，因此Xgboost无法直接处理离散特征(categorical feature)，需要数据预处理，要么labelEncoder转换为数值特征，当做连续值处理，要么one-hot编码，当做离散值处理。

- LightGBM则有对离散特征的单独处理，需要首先利用labelEncoder转换为数值，然后会利用[On Grouping for Maximum Homogeneity](http://link.zhihu.com/?target=https%3A//www.researchgate.net/publication/242580910_On_Grouping_for_Maximum_Homogeneity)提到的算法找到最优值，是从2^(n-1)-1个分区划分中选出最优optimal split，而不像OHE一样是n个划分，因此效果优于OHE。

  ![image-20200605155935227](http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2023-09-14-125900.jpg)
