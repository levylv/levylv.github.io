---
title: import及__init__.py
date: 2019-03-07 22:06:02
categories: [编程开发,Python]
---



## import注意事项:

```python
from package1 import module1
from package1.module2 import function1
from package2 import class1
from package2.subpackage1.module5 import function2
```

注意，module.function这种形式是不行的，用.前面只能是package





## __init__.py注意事项:

- 放在package目录下

- 可以没有，这种情况下一定要手动 from package import module

- 如果有，import package的时候，会自动加载该文件的内容，其中用法有包括:

  ```python
  __all__ = ['foofactories', 'tallFoos', 'shortfoos', 'medumfoos',
             'santaslittlehelperfoo', 'superawsomefoo', 'anotherfoo']
  # deprecated to keep older scripts who import this from breaking
  from foo.foofactories import fooFactory
  from foo.tallfoos import tallFoo
  from foo.shortfoos import shortFoo
  ```

  从而达到 from package import function的目的

