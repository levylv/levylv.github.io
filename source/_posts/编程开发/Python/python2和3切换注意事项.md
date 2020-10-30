---
title: python2和3切换注意事项
date: 2019-02-02 22:06:02
categories: [编程开发,Python]
---



- 除法问题。

  ```python
  # python2
  >>> 3/2
  1
  >>> 3/2.0
  1.5
  # python3
  >>> 3/2
  1.5
  >>> 3//2
  1.5
  ```

  

  **解决方法**：from __future__ import division

- 字符串编码不同：

  - python2字符串分为str类型和unicode类型

    - str：非unicode形式，以各种编码方式用字节存储(gbk, ascii, utf-8, utf-16等)
    - Unicode：unicode形式

    默认就是str类型，除非指定u"字符串"，才是unicode类型。相对来说不合理，在代码中涉及到中文的时候(用str类型)就会遇到编码和解码问题，中间有各种隐式的转换，程序可能报错，因为python2的默认编码为ascii，sys.getdefaultencoding()

  - pyhont3字符串分为str类型和bytes类型

    - str：unicode形式
    - bytes：非unicode形式，以各种编码方式用字节存储(gbk, ascii, utf-8, utf-16等)

    这样更加合理一点，我们平时用到的就是str类型，用最通用的Unicode来表示，只在底层存储时候转成相应编码方式。

  **解决方法**：文件头部指定编码utf-8，sys.setdefaultencoding为utf-8。

- map,filter等。python2返回结果是list，python3返回一个可迭代对象，需要list()才行

- python3没有xrange

- python2的raw_input() 等同于 python3的input()， python2的input()只接受数字输入

- print。 python2中的print是语句，python3是函数

  ```python
  # py2
  >>> print("hello", "world")
  ('hello', 'world')
  ```

  **解决方法**: from __future__ import print_function

- 还有个我自己发现的列表解析式的问题！ 

  ```python
  for i in range(6):
      y = [j for j in range(10)]
      print(j)
  ```

  这个代码，python2下 j = 9，但python3下j不存在！说明j的作用域只在列表解析式里。

