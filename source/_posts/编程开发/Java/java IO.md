---
title: java I/O
date: 2019-06-11 18:13:30
categories: [编程开发,Java]
---



1. 多线程（同步阻塞）；
2. IO多路复用（select，poll，epoll）（同步非阻塞，严格地来讲，是把阻塞点改变了位置）；
3. 直接暴露出异步的IO接口，如kernel-aio和IOCP（异步非阻塞）。

 

NIO就是IO多路复用，同步非阻塞，selector用来监听channel(OS内核空间)，另有ByteBuffer可读可写，处理程序数据空间。

![image-20200605165005503](https://tva1.sinaimg.cn/large/007S8ZIlly1gfhifritq9j30pd0elafo.jpg) 

字节流操作，无输入输出缓存，所以一般包一个bufferedInputStream，bufferedOutputStream

字符流操作，自带缓存。

 

https://www.zhihu.com/question/19732473

 

 

