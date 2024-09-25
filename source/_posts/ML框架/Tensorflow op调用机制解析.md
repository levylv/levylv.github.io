---
title: Tensorflow op调用机制解析
date: 2024-01-09
tags: Tensorflow
categories: ML框架
---



最近老是有需求去看tf一些op的源码实现，比如ftrl优化器源码实现，在看的过程中会发现涉及python和c++的调用逻辑，因为大部分op在tf底层其实使用c++实现的。因此，大致梳理了一下tf的op调用机制。



以FTRL优化器为例，探索这个调用链路。（本文中的Tf为1.12版本）

1. 原始import如下

```python
from tensorflow.train import FtrlOptimizer
```

本质上FtrlOptimizer所在包为`tensorflow.python.training.ftrl`，仔细看该py文件里的代码，发现其继承了Optimizer类，但是重写了若干方法，这些核心方法是优化器的具体实现，但是代码里是一个调用函数:

<img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2024-01-09-113955.png" alt="image-20240109193947724" style="zoom:80%;" />

2. training_ops

```python
from tensorflow.python.training import training_ops
```

该目录下存在`training_ops.py`文件，其代码如下

```python
from tensorflow.python.training.gen_training_ops import *
```

因此apply_ftrl其实来自gen_training_ops模块，而我们发现在该目录下并没有`gen_training_ops.py`文件，这里就涉及了bazel这个工具了

- Tensorflow采用了bazel(google自研，比makefile高效)来编译c++代码，具体bazel的原理可参考该[博客](https://blog.csdn.net/elaine_bao/article/details/78668657) 。核心在于各个目录下的BUILD文件。

像`gen_training_ops.py`这个文件，就是在training目录下的BUILD里定义的，在安装编译TF后才会生成的。BUILD如下所示，是一个自动生成python代码的方法tf_gen_op_wrapper_private_py。

![image-20240109201713354](http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2024-01-09-121717.png)

详细看其生成的代码：

<img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2024-01-09-124652.png" alt="image-20240109204650786" style="zoom:67%;" />

我们可以发现，其c++源码文件为`training_ops.cc`，因此我们搜索该文件后，可以发现存在两个文件：

- `tensorflow.core.ops.training_ops.cc`：该文件没有源码细节，只是做了op注册，后文会提到为什么要注册op。

  <img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2024-01-09-123808.png" alt="image-20240109203802741" style="zoom:80%;" />

- `tensorflow.core.kernels.training_ops.cc`：该文件则是真正的源码。

<img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2024-01-09-123846.png" alt="image-20240109203843919" style="zoom:80%;" />

由上，我们已经了解了如何去寻找真正的op源码，但是对于调用链路仍就有些困惑，即python是如何可以调用c++编译的动态库（关于动态库和静态库的解释，可以参考[资料](https://www.zhihu.com/question/20484931)），下文继续探索。

仔细看`gen_training_ops.py`文件，发现其调用的方法为`_op_def_lib._apply_op_helper('方法名', args)`

<img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2024-01-09-124737.png" alt="image-20240109204736609" style="zoom:80%;" />

继续追溯，其来源于```op_def_lib = _op_def_library.OpDefLibrary()```，再追溯，来自于`op_def_library.py`文件，`_apply_op_helper`方法核心代码为

![image-20240109205003728](http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2024-01-09-125005.png)

其中g是tf的Graph，也就是通过这个create_op方法来为Graph生成op进行调用。再继续追溯，其方法来自

```python
g = ops._get_graph_from_inputs(_Flatten(keywords.values()))
```

 来源为`tensorflow.framework.ops.py`文件，其中`_get_graph_from_inputs`返回的是一个Graph，而Graph类也在该文件，找到create_op方法：

<img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2024-01-09-125728.png" alt="image-20240109205727323" style="zoom:80%;" />

Operation同样在该文件，核心代码:

![image-20240109205927426](http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2024-01-09-125929.png)

`_create_c_op`同样在该文件，核心代码:

![image-20240109210042379](http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2024-01-09-130043.png)

再追溯c_api模块，我们发现其来自`tensorflow.python.pywrap_tensorflow.py`



3. pywrap_tensorflow

顾名思义，这个文件其实是python调用c++库的封装，打开文件，发现注释写着

```
"""A wrapper for TensorFlow SWIG-generated bindings."""
```

因此，其实这里tf采用了SWIG组件来实现python对c++动态库的链接，此外，在最新版本里，已经用了pybind11组件，关于这些组件的解释可以看[资料](https://www.zhihu.com/question/62208230),各组件的对比:

<img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2024-01-09-131231.png" alt="image-20240109211228949" style="zoom:67%;" />

对于SWIG的细节，这里就不再追溯了。调用链路大致可以理解为，通过SWIG来调用编译生成的`TF_NewOperation`进行Graph中的Node对象创建，而Node对象的创建需要上文提到的op注册:

<img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2024-01-09-131516.png" alt="image-20240109211514766" style="zoom:80%;" />

这里的细节也可以看[资料](https://blog.csdn.net/gaofeipaopaotang/article/details/80598840)









