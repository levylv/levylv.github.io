---
title: update_ops和control_dependencies的理解
date: 2023-07-10
tags: Tensorflow
categories: ML框架
---



## control_dependencies

作用是保证在执行之后的op时，确认某些op已经执行完毕，相加于强加了依赖。

常见的场景为: 当用了tf.assign之类的op，并不会实际执行，需用手动sess.run该op才行，为了方便起见，就可以引入control_dependdencies，例子如下:

```python
import tensorflow as tf
a_1 = tf.Variable(1)
b_1 = tf.Variable(2)
update_op = tf.assign(a_1, 10)
add = tf.add(a_1, b_1)

a_2 = tf.Variable(1)
b_2 = tf.Variable(2)
update_op = tf.assign(a_2, 10)
with tf.control_dependencies([update_op]):
    add_with_dependencies = tf.add(a_2, b_2)

with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    ans_1, ans_2 = sess.run([add, add_with_dependencies])
    print("Add: ", ans_1)
    print("Add_with_dependency: ", ans_2)

## 输出：
## Add:  3
## Add_with_dependency:  12
```





## update_ops

update_ops通常是针对需要保存训练过程中(不同batch)的变量，最常见的就是batch_normalization中的moving_mean和moving_std，源码里就是在每个batch里做assign，这些op则保存在update_ops中，很显然，update_ops会和上面提到的control_dependencies一起用，保证每个batch都会执行这些op。

