---
title: tensorflow的自动求导机制
date: 2023-12-20
tags: Tensorflow
categories: ML框架
---



参考资料:https://www.zhihu.com/question/54554389



自动求导分成两种模式，一种是 Forward Mode，另外一种是 Reverse Mode，目前基本机器学习库用的后一种。



Forward mode: 每一步都用到链式求导，每个节点都记录其对原始节点的导数。

Reverse mode: 每一步都只记录相邻节点之间的导数，然后最终计算loss到某个节点的导数时，只需将路径上的节点用链式法计算即可。
