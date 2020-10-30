---
title: 关于python2的编码问题
date: 2020-10-22
categories: [编程开发,Python]
---



## 基本知识

首先我们要弄明白Unicode这个概念，我以前一直理解有问题，以为是类似于ascii、utf-8的一种编码方式而已，然而我错了，可以看这篇文章：[字符编码笔记：ASCII，Unicode 和 UTF-8](http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)，总结一下就是：

- Unicode不是一种编码方式，而是定义了统一的字符集，包含了世界上的所有字符。它相当于维护了一个映射code point，一个二进制表示对应一个字符。虽然从这个理解上，也有点像是编码，毕竟也要用一个大概四个字节的二进制表示来表示一个字符，但还是要认为他只是一个字符集，而不是编码方式。
- UTF-8则是Unicode具体的一种编码实现方式，采用变长的字节来存储每个字符。
- 任何字符都可以用unicode表示，然后用某种编码方式，例如utf-8、ascii、gbk等方式，存储为字节形式。



## Python中的编码问题

- 在python2里面，str类型是以某种编码方式存好的字节，unicode类型才是以无编码方式存储的。

```python
>>> demo = '人'
>>> demo
>>> '\xe4\xba\xba'  # 3个字节
>>> demo = u'人'
>>> demo
>>> u'\u4eba' # 1个中文字符, 2个字节
```

- 为了避免一系列蛋疼的编码问题，我们可以采用这种方式：

  - 文件头部指定默认编码方式，这样str类型默认是按照utf-8来编码

    ```python
    # -*- coding:utf-8 -*-
    ```

  - 指定python解释器的默认编码方式，这样中间有一些隐式的编解码转换，就不会出现蛋疼的问题了

    ```python
    import sys
    reload(sys)
    sys.setdefaultencoding('utf-8')
    ```

    