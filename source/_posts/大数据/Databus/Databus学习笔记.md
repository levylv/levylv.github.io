---
title: Databus学习笔记
date: 2020-11-10
categories: [大数据,Databus]

---



Databus是一个低延迟、可靠的、支持事务的、保持一致性的数据变更抓取系统。



这块我也只是大概了解了一下，本质上想要解决的问题是：如果有一个mysql存储和一个redis缓存，mysql的变更要同步到redis里，为了保持数据的一致性，中间就加了一个databus。



图片解释：

<img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2023-09-14-125448.jpg" alt="image-20201110213021835" style="zoom:50%;" />

变为：

​              											<img src="http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2023-09-14-125452.jpg" alt="image-20201110213058927" style="zoom:50%;"                  />

​       
