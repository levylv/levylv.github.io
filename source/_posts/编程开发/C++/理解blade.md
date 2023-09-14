---
title: 理解blade
date: 2021-02-03
categories: [编程开发,C++]

---



今天在研究一个大型c++工程如何做单元测试，这个工程是用blade进行构建的，之前不是特别懂blade，看了网上的相关博客以及github的wiki，大致明白了blade原理，确实相比makefile而言，blade构建是更加方便简洁的。

参考相关博客：[blade学习](https://www.bbsmax.com/A/MAzAOWYq59/) , [blade git wiki](https://github.com/chen3feng/blade-build/blob/master/doc/zh_CN/develop.md) ，[blade cc wiki](https://github.com/chen3feng/blade-build/blob/master/doc/zh_CN/build_rules/cc.md)



具体不解释了，主要记录几个关键点：

- cc_library就是要打包的库，默认打包成静态库，也就是libxxx.a文件，也可以打包成动态库，也就是libxxx.so文件，通过BUILD配置即可。
  - 静态库直接将库文件打包到最终的可执行文件中。
  - 动态库只在可执行文件运行时候进行链接，需要建立一个lib文件夹放这些so库。
- blade的根目录往往是放在项目的上一级，因为项目依赖其他的git项目等，都需要在根目录下生成。blade会基于项目build的target，去其他目录查找所有的依赖target，构建拓扑顺序，最终全部生成target，默认是在bluid_release目录下。
- 对于一些项目依赖的第三方库，往往都是已经编译好的libxxx.a文件，这种的话需要注意几个点：
  - 需要有include目录，里面是一些头文件，方便编译找到定义，有点类似接口的感觉。
  - BUILD里面不需要再构建
  - 例如下面这个配置
    - prebuild=True，预先已经生成了libjemalloc.a，不需要再生成
    - export_incs：这个就是定义编译时候寻找头文件的目录，已经编译好的第三方库会有这个include目录放头文件。正常我们自定义工程是不需要加这个配置的，因为头文件是在整个自定义工程里有的。
    - build_dynamic：生成动态库

![image-20210203173926774](http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2023-09-14-122129.jpg)

