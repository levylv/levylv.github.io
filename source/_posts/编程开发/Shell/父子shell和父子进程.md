---
title: 父子shell和父子进程
date: 2019-02-15 18:13:30
categories: [编程开发,Shell]
---

首先，shell也是一个进程！

 

举个例子：

在某个shell下(该shell也是一个进程，id为1)，执行一个脚本文件，同时便会生成一个非交互子shell（进程id为2），然后该脚本文件的每一行可执行程序，又会生成新的子进程id如3，4，5，6等等

 

 

父子shell中的变量问题：

- 普通自定义变量不会共享，只在当前shell生效

- 环境变量：

   如果没有切换用户，子shell（非交互式）共享父shell的环境变量

   如果切换了用户:

  - 假设是su，那会创建子non-login shell（交互式），环境变量部分新用户，部分原用户，部分消失(只加载~/.bashrc)

  - 假设是su -，那会创建login shell（交互式），环境变量全部为新用户(加载/etc/profile,~/.bash_profile)

 

 