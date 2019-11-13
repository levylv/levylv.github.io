---
title: 'Tensorflow中的变量作用域和名称作用域'
date: 2017-07-18 22:06:02
tags: Tensorflow
---

在定义复杂网络时，为了让变量和操作更加清晰，我们需要用作用域scope来为变量或者操作加前缀。在Tensorflow中，总共有以下几个定义域操作：

```python
tf.name_scope()
tf.op_scope()
tf.variable_scope()
tf.variable_op_scope()
```
主要可以分为两类：变量作用域和名称作用域。其中`tf.name_scope`和`tf.op_scope`都属于名称作用域，这两者的唯一区别就是values的参数位置不同：


```python
tf.name_scope(name, default_name=None, values=None)
tf.op_scope(values, name, default_name=None)
```

`tf.variable_scope`和`tf.variable_op_scope`都属于变量作用域，两者区别跟上面是类似的。

那么，变量作用域和名称作用域又有什么区别呢？

对于名称作用域，顾名思义，就是一旦定义了该作用域，该代码块中的**变量和操作**，全部会加上该作用域前缀，若作用域名称相同，则名称后缀自动加一。但是，也有一个例外就是`tf.get_variable()`这个操作。

`tf.get_variable()`有以下的性质：
- `tf.get_variable('name', ...)` 中的name是无视名称作用域的，也就是说，即使该操作在某名称作用域中，name也不会加上相应前缀。
- 不同于`tf.Variable()`通过直接获取值来初始化，`tf.get_variable()`则是通过变量名以及定义初始化分布来进行初始化，若变量名已经存在，那么程序就会报错，相反，如果对于`tf.Variable()`，我们定义了相同的变量名，则程序会在变量名后缀上自动加一。


从某种意义上说，变量作用域`tf.variable_scope`就是为了`tf.get_variable`而设计的。

- `tf.get_variable('name', ...)`中的name会自动加上变量作用域的后缀。
- 变量作用域可以设定`reuse = True`，从而定义相同名字的变量为共享变量，若名字不同，则会报错。个人认为，定义变量作用域以及该操作都是为了实现共享变量的功能。
- 变量作用域还可以为`tf.get_variable('name', ...)`设置默认的初始化分布！
- 最重要的一点是，**变量作用域`tf.variable_scope('name')`一旦开启，也就相当于间接开启了一个名称作用域`tf.name_scope('name')`！**

总结来说，这些设计的目的大概这样的：

- 为了区分变量和操作，定义了名称作用域。
- 名称作用域和普通的变量创建操作有重复名称自动后缀加一的特性，因此无法实现变量共享，于是定义了`tf.get_variable`操作。
- `tf.get_variable`操作也需要加前缀，于是定义了变量作用域。变量作用域相当于加强版的名称作用域！

最后，给个测试代码：

```python
with tf.variable_scope("foo"):
    v = tf.get_variable("v", [1])
    x = 1.0 + v
assert v.name == "foo/v:0"
assert x.op.name == "foo/add"
assert x.name == "foo/add:0"

with tf.variable_scope("foo", reuse = True):
    with tf.name_scope("bar"):
        v = tf.get_variable("v", [1])
        x = 1.0 + v
        y = 1.0 + x
assert v.name == "foo/v:0"  # 共享变量
assert x.op.name == "foo_1/bar/add"  # foo此时为名称作用域，重复定义，需要加一
assert x.name == "foo_1/bar/add:0"
assert y.op.name == "foo_1/bar/add_1" # add操作重复定义，自动加一


# 还有一个关于名称作用域和变量作用域嵌套的问题，若使用对象而非字符串开启作用域，则该作用域不嵌套

with tf.name_scope('t1') as scope:
    with tf.name_scope('t2'):
        with tf.name_scope(scope):
            x = tf.Variable(1)
assert x.op.name == 't1/Variable'

with tf.name_scope('t1') as scope:
    with tf.name_scope('t2'):
        with tf.name_scope('t3'):
            x = tf.Variable(1, name = scope) # 这样也不嵌套
assert x.op.name == 't1_1'
```

关于`tf.variable_scope()`和`tf.get_variable()`的更多操作，可以看[共享变量-极客学院Wiki](http://wiki.jikexueyuan.com/project/tensorflow-zh/how_tos/variable_scope.html)


