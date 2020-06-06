---
title: POJO、JavaBean和SpringBean
date: 2020-05-09 18:13:30
categories: [编程开发,Java]
---

## POJO
普通JAVA类，专指只有setter / getter / toString的简单类，包括DO/DTO/BO/VO等。

POJO是一个简单的、普通Java对象，特点是有private的属性和public的getter、setter，除此之外不具有任何特殊角色，不继承或不实现任何其它Java框架的类或接口。 

## JavaBean

**为了方便开发，约定的一种java类的规范！**，
- 所有属性为private。
- 这个类必须具有一个公共的(public)无参构造函数
- private属性必须提供public的getter和setter来给外部访问，并且方法的命名也必须遵循一定的命名规范。 。
- 这个类应是可序列化的，要实现serializable接口。

没有其他业务逻辑的javaBean,就是一个POJO

## SpringBean

SpringBean是受Spring管理的对象。所有能受Spring容器管理的对象，都可以成为SpringBean。


Spring容器对Bean没有特殊要求，不像JavaBean 一样遵循一些规范（不过对于通过设值方法注入的Bean,一定要提供setter 方法。）

传统的的Javabean，如果我们要创建一个 Bean，我们就要使用关键字 New。
但是，在 Spring 中，Bean 的创建是由 Spring 容器进行的，也就是说，在 Spring 中使用 Bean 的时候，不是由关键字 New 来创建实例了, 由Spring容器管理其生命周期行为，