---
title: shell的变量
date: 2019-07-02 18:13:30
categories: [编程开发,Shell]
---

### 局部变量

只在函数内定义的变量，如果不加local关键词，其实还是一个全局变量(函数外能够访问)

 

### 全局变量

在当前shell内定义的变量，其他shell不能访问

 

### 环境变量

用export定义，在当前shell的子shell中可以传递，传子不传父。

