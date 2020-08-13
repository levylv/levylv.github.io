---
title: Tensorflow和Pytorch的比较
date: 2020-07-29
tags: [Tensorflow,Pytorch]
categories: ML框架
---



### 比较

来自：https://zhuanlan.zhihu.com/p/110177607

Tensorflow 在工业界有着更广泛的应用，Pytorch在学术界有着更广泛的应用。

TensorFlow强在在线部署，多语言支持和较好的线上系统稳定性，我觉得这个是TensorFlow在业界被广泛应用的最主要原因。在业界，无论算法性能有多好，总归还是要上线的，不然都是白扯，不上线就算不到kpi中去。而方便部署到线上，支持多语言，并且有较好的系统稳定性以及有非常多线上应用实例是TensorFlow称霸业界的主要原因。

比较好一点的公司会在线上部署TensorFlow Serving，这样算法科学家们只需要提供相应的Tensorflow model即可将模型上线。但是据我所知，在国内某电商巨头公司内部，只有某院才能够使用TensorFlow Serving, 其他业务部门(包括很多小公司)想将TensorFlow modle上到线上，一律通过TensorFlow api来加载模型，做线上serve。这时候TensorFlow 支持多语言的特性就发挥了巨大的作用，它不仅支持c++， python，还支持如java等语言；很多公司目前的infrastructure 还没有针对AI做更新换代，也不提供相应的TensorFlow serving 能力，所以这些公司基本都用C++或者java来做TensorFlow 模型的线上serve。再者，TensorFlow基于静态图的这种方式非常适合线上应用，一次build好graph，多次在这个图上面做inference，工程效率很高。

但是在线下，TensorFlow体验很差，用过的人都觉得体验很不好。1. 在TensorFlow1.x时期，基于静态图的模式，无法在变量定义和使用的时候查看tensor的值，只有在session run的时候才能查看tensor的值，使得debug起来异常困难。2. TensorFlow API混乱，使用体验比较差。

而Pytorch 在API和文档方面都做得更好，并且pytorch是基于动态图的模式，可以很方便的查看tensor的值，可以像debug Python程序一样debug pytorch模型。因此，pytorch在只关心算法性能而不用太关心部署的学术界，非常受欢迎。

虽然TensorFlow2.x默认是eager execution模式，可以像pytorch一样直接查看tensor的值，但是个人感觉出来的还是有点晚，短时间内可能不太会是主流的线下框架了。一方面，已经习惯TensorFlow1.x的人还要重新学习tensorflow2.x，并且在TensorFlow1.x中静态图习惯的一些写法，在TensorFlow2.x可能不怎么支持，还要重新再踩很多坑，才能比较好的掌握；另一方面，习惯使用pytorch的那波人不太会去在学习TensorFlow2.x。

虽然Pytorch在新版本中也出了新的特性，更好的支持线上部署，但毕竟是才出来不久，没有经过线上的长时间检验，实际线上应用案例和分享也比较少。而TensorFlow自15年release以来，已经在线上经历了五年多的考验，撑过了多次双十一高流量的检测，性能稳定。Pytorch想更广泛应用在线上，短时间内也不太现实，替换成本还是很高，TensorFlow在这方面已经占尽了先机。

总的来说，Tensorflow在业界线上部署占尽了先机，而Pytorch在线下占尽了先机，短时间内谁都不能很快的取代谁。



### 现在较好的部署手段

##### 使用Tensorflow

- TF-serving
- Tensorflow java/C++/python api，自定义服务上线
- 转成uff模型，tensorRT上线 

##### 使用Pytorch

- Pytorch python等api，自定义服务上线
- 转成tensorflow模型，然后参考tensorflow上线方式
- 转成onnx模型，tensorRT上线



TensorRT：https://zhuanlan.zhihu.com/p/88318324