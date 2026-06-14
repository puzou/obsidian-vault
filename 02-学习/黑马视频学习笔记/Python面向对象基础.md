---
tags:
  - 学习笔记
  - 黑马
source_type: 视频+PPT
source_title: 黑马程序员大模型Python进阶 — Day01 面向对象基础
source_url:
date: 2026-05-15
---

# Python面向对象基础

## 资料信息

| 字段 | 内容 |
|------|------|
| 类型 | 视频 + PPT + 随堂笔记 |
| 标题 | 黑马程序员大模型Python进阶 — Day01 Python面向对象基础 |
| 来源/链接 | B站黑马程序员课程 |
| 作者/UP主 | 黑马程序员 |

## 核心要点

1. Python `__str__` 和 `__repr__` 是两个不同的魔法方法，触发场景不同
2. Python `__del__` 不是 C++ 析构函数，调用时机不确定
3. Python 多继承 MRO 用 C3 线性化算法，不只是"就近原则"那么简单

## 详细笔记

### `__str__` vs `__repr__`

这两个方法都控制对象的字符串表示，但触发场景不同：

| | `__str__` | `__repr__` |
|---|---|---|
| **触发场景** | `print(obj)` / `str(obj)` | 交互式直接输出 / `repr(obj)` / 容器内显示 |
| **目标受众** | 用户（可读性好） | 开发者（信息明确，最好能 eval 还原对象） |
| **未定义时** | 回退调用 `__repr__` | 显示 `<ClassName object at 0x...>` |

**关键规则：只定义了 `__repr__` 但没定义 `__str__` 时，`print(obj)` 也会走 `__repr__`。反过来不行。**

```python
class Car:
    def __init__(self, brand):
        self.brand = brand

    def __str__(self):
        return f'{self.brand}'           # 面向用户，简洁可读

    def __repr__(self):
        return f"Car('{self.brand}')"    # 面向开发者，能 eval 还原
```

```python
>>> c = Car('BMW')
>>> print(c)       # 触发 __str__
BMW
>>> c               # 交互式触发 __repr__
Car('BMW')
>>> print([c])      # 容器内触发 __repr__
[Car('BMW')]
```

> 课程只讲了 `__str__`，没提 `__repr__`。实际开发中 `__repr__` 更重要，建议两个都写。

### `__del__` 不是 C++ 析构函数

C++ 的析构函数在对象离开作用域时**立即调用**，Python 的 `__del__` 不是：

| | C++ 析构函数 | Python `__del__` |
|---|---|---|
| **调用时机** | 对象离开作用域时立即调用 | 垃圾回收时调用，时机不确定 |
| **确定性** | 确定 | 不确定 |
| **用途** | 释放资源（文件、内存） | 不建议用于关键资源释放 |

**坑点：**
- Python 用引用计数 + 分代 GC，`__del__` 调用时机取决于 GC
- 循环引用时，`__del__` 可能延迟甚至不被调用
- 程序退出时也不保证所有 `__del__` 都执行

**正确做法：**需要确定性释放资源，用 `with` 语句（上下文管理器 `__enter__` / `__exit__`），不要依赖 `__del__`。

```python
# 不要这样
class FileHandler:
    def __init__(self, path):
        self.f = open(path)
    def __del__(self):
        self.f.close()       # 不可靠！

# 要这样
class FileHandler:
    def __enter__(self):
        self.f = open(self.path)
        return self.f
    def __exit__(self, *args):
        self.f.close()       # 确定会执行

with FileHandler('data.txt') as f:
    content = f.read()
```

### 多继承与 MRO（方法解析顺序）

课程讲了多继承的"就近原则"——`class A(B, C)` 优先用 B 的方法。但实际更复杂：

**C3 线性化算法**决定 MRO 顺序，规则：
1. 子类优先于父类
2. 按声明顺序从左到右
3. 单调性：不改变局部优先顺序

```python
class A:
    def test(self): print('A')

class B(A):
    def test(self): print('B')

class C(A):
    def test(self): print('C')

class D(B, C):
    pass

d = D()
d.test()   # 输出 B（就近原则，B 在前）

# 但看 MRO 完整链：
print(D.mro())
# [D, B, C, A, object]
# 不是 [D, B, A, C, object]——C 排在 A 前面
```

**菱形继承时**，C3 线性化保证每个类只出现一次，且顺序合理。如果出现冲突（不满足单调性），Python 会直接报错拒绝创建类。

**查看 MRO 的方式：**
```python
ClassName.mro()       # 返回列表
ClassName.__mro__     # 返回元组
```

> 实际开发中多继承用得少，但了解 MRO 对排查方法调用问题很有帮助。

### 其他需要注意的 Python OOP 细节

**1. 类定义的三种写法等价**

```python
class Car:           # 推荐，最简洁
class Car():         # 可以，括号多余
class Car(object):   # Python 2 兼容写法，Python 3 中效果一样
```

**2. Python 属性可以动态添加（C++/Java 不行）**

```python
c = Car()
c.color = '红色'     # 类外随时加属性，只有 c1 有，c2 没有
```

课程中演示了这个特性，说明如果不写 `__init__`，属性只在类外赋值后才有，且独属于该对象——其他对象访问会报 `AttributeError`。

**3. self 是显式的（C++ this 是隐式的）**

Python 方法第一个参数必须写 `self`，调用时自动传入，不需要手动传：
```python
c.run()    # 等价于 Car.run(c)，self 自动接收 c
```

## 关键概念

| 概念 | 解释 |
|------|------|
| `__str__` | print(obj) 时自动调用，返回用户友好的字符串 |
| `__repr__` | 交互式/容器内显示时调用，返回开发者友好的字符串；未定义 __str__ 时会回退到此 |
| `__del__` | 对象被 GC 回收时调用，时机不确定，不能当 C++ 析构用 |
| `__init__` | 创建对象时自动调用，用于初始化属性（C++ 构造函数） |
| MRO | Method Resolution Order，多继承时方法查找顺序，C3 线性化算法 |
| self | 类似 C++ 的 this / Java 的 this，但必须显式写在参数列表 |

## 疑问与待深入

- `__repr__` 和 `__str__` 在 Django/Flask 等框架中的实际使用场景？
- C3 线性化算法在什么情况下会报错？

## 与我的关联

- 写 Python 项目时经常用 `__init__` 和 `__str__`，但之前没注意 `__repr__` 的区别
- 特征匹配 benchmark 项目中可能有需要 `__repr__` 调试的场景
