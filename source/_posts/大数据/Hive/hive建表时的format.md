---
title: hive建表时的format
date: 2018-05-30 18:13:30
categories: [大数据,Hive]
---

hive建表时有三种指定，举例如下：

- ROW FORMAT SERDE 'org.apache.hadoop.hive.ql.io.orc.OrcSerde'
- STORED AS INPUTFORMAT 'org.apache.hadoop.hive.ql.io.orc.OrcInputFormat'
- OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.orc.OrcOutputFormat'

 

对于第一种指定row format delimited，是指hive读取数据文件时的分隔符，用来划分字段，默认分隔符是'\001'。加入serde的话，是指定序列化和反序列化的一些规则，这个我就不深究了。。

 

对于第二种指定store as inputformat，就是hive加载数据时，对数据文件的处理，这里可以自定义一些方法，比如原来的数据文本分隔符是'|'，我的表指定的是'\t'，那么可以自定义方法进行正则化处理。

 

同理对于第三种指定output，就是hive输出数据时，对数据文件的处理，也可以自定义一些方法。这里注意，输入和输出的数据文件是统一的。

 

值得注意的是，无论指定何种分隔符，hive -e "select * from table " > x，x文件的分隔符都是'\t'(因为hive环境中的字段是以\t分割的，hive环境和数据文件类似于映射的关系，如果是beeline那就是|)，在excel中打开都是一格（excel需要以逗号分割csv）。如果想要以指定分隔符生成x，那么可以

```sql
insert overwirt local directory x

row format delimited fields terminated by '分隔符'

select * from *
```



﻿**那么对于上述举例中的三种指定，含义就是将数据文件转化为了一种更高效，压缩更好的orc文件，这个数据文件我用cat查看了一下是乱码的。。**

 

最后总结一下 平时建表的一些规范：

- 如果这个表纯粹是各个数据表之间的处理，那么建议写成举例形式，orc文件毕竟高效。
- 如果这个表是从外部生成数据导入，例如python处理后生成数据导入，那么生成表是要指定分隔符： ROW FORMAT DELIMITED FIELDS TERMINATED BY '分隔符'，下面两种指定不加。

 