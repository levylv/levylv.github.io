---
title: Spark中的UDF
date: 2020-08-13
categories: [大数据,Spark]
tags: 模型上线
---



我们做模型想要上线，很多时候都会借助spark的udf来实现，最近在摸索这个东西，有了一点心得记录一下。



### Hive/Spark SQL中的UDF

这种上线方式是经常会用到的，UDF分为三种：

- UDF：一进对应一出
- UDTF：一进对应多出，经常遇到的就是比如一行数组数据分为多行，类似explode lateral view
- UDAF：多进对应一出，就是聚合函数，类似sum()，count()



UDF写完后直接在SQL中使用即可，一般写法又分为java和python两种：

- Java：这种方式比较常见，引用hadoop/spark接口即可，最终形成一个jar包，可以包含多个模块，容易管理，借助maven工具也容易进行依赖管理。
- Python：transform(..)  using(..)的方式py的好处是线下代码直接迁移，不用大改(一般模型也是用python开发巨多)，但坏处也有：
  - 依赖py文件的问题：**py文件无法打包**，模块之间不能复用，不太容易管理。依赖的py文件需要手动add file上去。
  - 依赖lib的问题：如果依赖第三方包，需要**自定义python环境**，然后打包上传，运行时选择自定义python环境。http://heloowird.com/2018/01/29/hive_python_udf/



### Spark中的UDF

对于复杂的模型，例如需要特征工程之类的，我们可能就需要直接写spark代码来实现。如果核心算法有现成的spark包，例如xgboost这种还好说，如果是自己手写实现的算法，则需要将其包装成udf来实现分布式处理。

UDF也是支持是三种（这个我是基于pyspark的pandas udf来的）：

- 一对一的scalar

- 多对多的Grouped Map

- 多对一的Grouped Aggregate

  

同样的，这种方式也有java(确切来讲是scala)和python两种方式：

- Scala(Spark)：spark.udf.register(*)，基本原理是实现一个function，然后注册成udf。function里的计算就是普通的计算。
- Python(Pyspark)：原理也是类似，有udf功能。更进一步的，spark 2.4.0之后提出了pandas udf，基于apache arrow，借助pandas向量计算的能力大幅提升计算性能，同时省去了py4j的序列化流程。https://www.jianshu.com/p/17117574a86b。python的坏处也和sql udf里一样，依赖问题比较蛋疼。



##### 注意事项

- UDF里的变量都是普通变量，不能是rdd，因为rdd是不能嵌套的，每个executor都在做udf计算，再来一个rdd，executor又得分发，这是不对的。
- 如果想要从hdfs里获取某个文件分发给executor做udf计算，则需要在driver里先获取该文件，然后将其变为普通变量，用sc.broadcast将其分发到每个executor上。