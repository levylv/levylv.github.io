---
title: C++虚函数和Java类比
date: 2021-05-31 
categories: [编程开发,C++]
---

小结：

<img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2023-09-14-122149.jpg" alt="image-20210531170324424" style="zoom:50%;" />

- 在C++中，多态是靠虚函数实现的，因为如果是普通函数，调用的方法是根据当前指针类型来判断的，而不是根据指针所指向对象的类型，JAVA则是根据实际对象分配的，所以JAVA的普通函数就类似C++的虚函数。
- 所以C++如果一个类是基类，它的析构函数一定是虚函数。
- C++的纯虚函数就类似JAVA的抽象函数，也就是只有函数定义。
- C++的抽象类就是JAVA的抽象类，也就是有至少有一个纯虚函数/抽象函数的类。
- C++的虚基类就是JAVA的接口，也就是全部是纯虚函数/抽象函数的类。
- C++可以多继承，而JAVA只能单继承，所以JAVA又搞了接口出来。



参考资料：

[C++ 与 Java 之中的虚函数、抽象函数、抽象类、接口 比较](https://blog.csdn.net/u013630349/article/details/50838558)

[C++虚函数与JAVA中抽象函数比较](https://www.huaweicloud.com/articles/6b9f1996380f9e89335a747ac42322a5.html)



