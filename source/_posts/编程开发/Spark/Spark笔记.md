---
title: Spark笔记
date: 2018-10-18 22:06:02
categories: [编程开发,Spark]
---



- 如何手动分区：
  分两种情况，创建 RDD 时和通过转换操作得到新 RDD 时。

  对于前者，在调用 textFile 和 parallelize 方法时候手动指定分区个数即可。例如 sc.parallelize(Array(1, 2, 3, 5, 6), 2) 指定创建得到的 RDD 分区个数为 2。

  对于后者，直接调用 repartition 方法即可。实际上分区的个数是根据转换操作对应多个 RDD 之间的依赖关系来确定，窄依赖子 RDD 由父 RDD 分区个数决定，例如 map 操作，父 RDD 和子 RDD 分区个数一致；Shuffle 依赖则由分区器（Partitioner）决定，例如 groupByKey(new HashPartitioner(2)) 或者直接 groupByKey(2) 得到的新 RDD 分区个数等于 2。



- 集团的hadoop，hive，spark环境：
  - 集群在线上环境中，线下环境与之没有打通，无法连接。
  - hive有个hivejdbc接口，jdbc:hive2://10.83.16.36:8083，线下环境可以访问该ip，由此访问线上hive。
  - 想要客户端访问集群，只能通过客户端，也就是各类中转机，堡垒机，D++等，同样需要装上hadoop，hive，spark等环境，并且做相关配置，例如hadoop的core-site，yarn-site，mapred-site.xml，以及spark的hive-site等。

 

- 关于版本问题：
  - maven的pom指定了编译打包版本(spark,java)，也是本地调试运行时的版本，此时甚至不需要本地安装java或者spark，因为pom会自动拉取该程序文件到仓库。
  - 打成jar包后我们可能会到处部署，放到不同机器环境中，此时运行版本最好与编译版本保持一致，或者高于编译版本(java向下兼容)
  - 对于hadoop/spark集群，无论master/slave所有机器上都要安装hadoop/spark，并且保持配置一致。
  - 所以为了统一，maven的pom指定版本要与集群上安装版本一致。
  - maven clean package只是将编译产生的类打包，单独部署可能无法运行，适合作为工具包被导入；maven clean assembly:assemby还将所有依赖等文件全部打包，可以单独部署运行，最终jar包也很大，要执行这一操作需要在pom里加入assembly插件。

 

 

- spark优化相关
  - spark的设置：程序代码 > spark-submit指定 > spark-default.xml
  - shuffle.partition最好设置成自动调整，使每个分区的数据尽量平均
  - 数据倾斜是难以避免的，即使设置partition也没用，此时可以用map/broadcast join来避免shuffle操作，同时还有其他的一些手段例如随机前缀以及采用倾斜key等。

 

- spark基础操作：
  - transformation: rdd -> rdd， 注意：reduceByKey, sortByKey等都是该类操作。
  - action： rdd 返回结果给drive problem， 注意：foreach， collect, show, count, countByKey,createOrReplaceTempView是该类操作

 

- spark rdd处理不能嵌套！例如在对一个rdd做操作的时候，不能引入另一个rdd的操作。所以citys(Dataframe类型).foreach(func(_, data(也是Dataframe类型)))这种想法是不行的。 直观来说，就是对有分区的数据的处理时，不能再传有分区的数据。但如果单纯想要传rdd的话(不对这个rdd再做操作)，也是可以的，用broadcast变量。

 

 

- rdd.foreach(x => func(x, y)) 对于这类操作，func和y都需要支持序列化和反序列化。
  - 变量y可以定义成broadcast变量，从而省去每个task序列化和反序列化的过程中，对变量y的序列化和反序列化，直接广播到了每个executor上。
  - 还要注意的是，这里的func也要支持序列化，如果是自定义的func，建立放在一个支持序列化的类中。也可以直接将func写在foreach中。
  - 如果func，y是引用了某个类的成员函数或变量，那么这整个类都要支持序列化，所以最好放在当前函数中。

 

- 出现“org.apache.spark.SparkException: Task not serializable"这个错误，一般是因为在map、filter等的参数使用了外部的变量，但是这个变量不能序列化。特别是当引用了某个类（经常是当前类）的成员函数或变量时，会导致这个类的所有成员（整个类）都需要支持序列化。解决这个问题最常用的方法有：

1. 1. 如果可以，将依赖的变量放到map、filter等的参数内部定义。这样就可以使用不支持序列化的类；
   2. 如果可以，将依赖的变量独立放到一个小的class中，让这个class支持序列化；这样做可以减少网络传输量，提高效率；
   3. 如果可以，将被依赖的类中不能序列化的部分使用transient关键字修饰，告诉编译器它不需要序列化。
   4. 将引用的类做成可序列化的。

 

- spark的worker, executor, core的概念：
  - worker就是集群里可执行的机器，一个worker可以有多个executor。
  - 一个executor就是CPU，一个CPU可以有多个核。
  - 一个core(核)对应一个线程，也就是一个task，一个核同时只能执行一个task。注意，这个的core不是指物理核，是虚拟核。
  - 一般来讲几个物理核就是几个线程，但是通过超线程技术，一个物理核可以分成多个虚拟核，从而使得一个核可以有多个线程。但是，一个核同时只能执行一个线程。
  - 并发：多个线程在单个核心运行，同一时间一个线程运行，系统不停切换线程，看起来像同时运行，实际上是线程不停切换。
  - 并行：多个线程在多个核心运行，线程同时运行。

 

 

 

 