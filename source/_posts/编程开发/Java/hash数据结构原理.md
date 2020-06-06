---
title: hash数据结构原理
date: 2020-03-08 18:13:30
categories: [编程开发,Java]
---

java hashMap底层实现：数组+链表+红黑树(链表长度大于8的时候)

hashcode 通过hash函数 —> hash值

hash值 通过数组长度取余 —> index

 

所以说hash函数要尽量能够均匀数组位置。

java里的hashMap的hash函数是：Hash值=（hashcode）^ (hashcode >>> 16)

hashMap预设数组长度也蛮重要，到load_factor(默认0.75)后，会resize 2倍，index都得重新算了。

 

python里dict对string的hash函数就很蛋疼，是64位的整数。

  

虽然hash的查询，插入，删除都很快，因为hash里的数组填不满，所以说hash相对其他数据结构而言，比较耗内存，且resize很慢。有点像空间换时间。