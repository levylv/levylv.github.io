---
title: Java泛型
date: 2018-11-19 18:13:30
categories: [编程开发,Java]
---

```java
List stringArrayList = new ArrayList();

List integerArrayList = new ArrayList();

Class classStringArrayList = stringArrayList.getClass();

Class classIntegerArrayList = integerArrayList.getClass();

if(classStringArrayList.equals(classIntegerArrayList)){ Log.d("泛型测试","类型相同"); }  --相同
```

 

**泛型类型在逻辑上看以看成是多个不同的类型，实际上都是相同的基本类型。**

```java
public void showKeyValue1(Generic obj){ Log.d("泛型测试","key value is " + obj.getKey()); }

Generic gInteger = new Generic(123);

Generic gNumber = new Generic(456);

showKeyValue(gNumber);

// showKeyValue这个方法编译器会为我们报错：Generic<java.lang.Integer>

// cannot be applied to Generic<java.lang.Number>

// showKeyValue(gInteger);

```

**同一种泛型可以对应多个版本（因为参数类型是不确定的），不同版本的泛型类实例是不兼容的**。