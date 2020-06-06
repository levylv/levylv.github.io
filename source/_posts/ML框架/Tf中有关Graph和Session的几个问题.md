---
title: Tensorflow中有关Graph和Session的几个问题
date: 2017-07-15 22:48:55
tags: Tensorflow
categories: ML框架
---

晚上在写一个简单的cnn时遇到了一个编译错误，代码检查了半天没找到问题。。最后鬼使神差地把全局变量初始化语句改了一下，竟然编译通过了。。研究了一下原因，应该是默认graph的问题。于是，再加上之前的默认session，我把tensorflow中默认graph和默认session几个注意点总结一下。

# 默认Session问题

- session创建后，如果没有指定graph，则该session会调用默认的graph。
- 调用默认graph的话，session创建语句可以在文件任意位置。因为即使session放在前文，后文里若是定义了新的graph节点，这些节点也会加到默认graph中，接下来调用该session时，调用的也是新的默认graph。
- 如果以`sess = tf.Session()`创建session，则该session不会作为下文的默认session，需要以`with`语句开头调用该session后，才作为下文的默认session。如果以`sess = tf.InteractiveSession()`创建session, 则该session即是下文的默认session。**默认session的好处是可以直接使用`operation.run()`或`tensor.eval()`, 无需指定session来run**。
- `with`语句有个好处是，该代码块结束后，session会自动`close`。

# 默认Graph问题

- 如果不指定graph，创建的新节点都会加入到默认graph中。注意，该graph是一个**全局默认graph**,也就说如果你定义了一个函数，这个函数里增加了一些节点，那么，每次调用这个函数，都会在默认graph中增加新节点！因此，如果想要定义类来实现算法，那么以防这种情况，建议将所有的节点操作放在类的初始化`__init__`方法中，这样对于每个实例，初始化也只会执行一次而已。
- 有个要特别注意的节点操作`tf.global_variables_intializer()`。该项操作读取的是**当前默认graph中**的variable，如果在前文中定义就会出现问题！举个例子，我在前文中定义`init_var = tf.global_variables_intializer()`，然后中间加入新的variable, 最后再执行`init_var.run()`，这样就会出现编译错误信息`Attempting to use uninitialized value beta1_power`！也就是我今天遇到的编译问题！我们只能重新执行`tf.global_variables_intializer().run()`才行！


# 总结

其实上面说了那么多问题，其实只要规范好代码就可以避免上述问题。所谓的规范就是，session创建和variable初始化这两个步骤，都在graph定义完成后再执行！
