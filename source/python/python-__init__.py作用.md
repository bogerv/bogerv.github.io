---
title: __init__.pyt文件作用
date: 2020-12-05 11:01:50
categories: [python]
---

- 作为 Python 中 package 的标识
- 定义 "\_\_all\_\_" 用来模糊导入
- 做包提升操作

假设有如下项目结构:

```python
├─test
│  ├─package1
│  │  ├─__init__.py
│  │  ├─m1.py
│  ├─package2
│  │  ├─__init__.py
│  │  └─m2.py
│  ├─test.py
```

如上目录结构，如果两个包中的\_\_init\_\_.py 都为空的话，默认在 test.py 中我们只能执行如下

```python
如上目录结构，如果两个包中的__init__.py 都为空的话，默认在 test.py 中我们只能执行如下
```

当想要执行

```python
import package_2
print(package_2.m2.m2s())

或者

import package_2
print(package_2.m2.m2s())

会抛出异常
AttributeError: module 'package_2' has no attribute 'm2'
```

## 在 \_\_init\_\_.py 中进行包提升

在 package2 包中的添加

```python
from package2.m2 import m2s
```

在 test.py 中，执行

```python
from package2 import m2s

print(m2s())
```

添加 `__all__ = ['m2']` 则允许使用

```python
from package2 import *
print(m2.m2s())
```

> 当 `__init__.py` 中定义了 `__all__` 变量时，import \* 只能导入 `__all__`允许的模块
