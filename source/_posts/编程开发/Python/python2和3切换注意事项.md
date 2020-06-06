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

- unicode string. python2默认是ASCII码的字符串，UNICODE字符串则是另一种表示, 就是unicode和ascii没统一：

  ```python
  >>> str = "我爱北京天安门"
  >>> str   #本身是UNICODE编码的字符，只能转成底层的ASCII的二进制形式来表示
  '\xe6\x88\x91\xe7\x88\xb1\xe5\x8c\x97\xe4\xba\xac\xe5\xa4\xa9\xe5\xae\x89\xe9\x97\xa8'
  >>> str = u"我爱北京天安门"
  >>> str  #指定这是一个UNICODE编码的字符，就用底层的UNICODE的二进制形式来表示
  u'\u6211\u7231\u5317\u4eac\u5929\u5b89\u95e8'
  ```

  而python3底层统一用utf-8来表示。

  **解决方法**：其实只是存储方式的不同，print什么的都不影响。

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

