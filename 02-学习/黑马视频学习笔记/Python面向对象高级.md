---
tags:
  - 学习笔记
  - 黑马
source_type: 视频+PPT+随堂笔记
source_title: 黑马程序员大模型Python进阶 — Day02 面向对象高级
source_url:
date: 2026-05-27
---

# Python面向对象高级

## 资料信息

| 字段 | 内容 |
|------|------|
| 类型 | 视频 + PPT + 随堂笔记 |
| 标题 | 黑马程序员大模型Python进阶 — Day02 Python面向对象高级 + 综合案例 |
| 来源/链接 | B站黑马程序员课程 |
| 作者/UP主 | 黑马程序员 |

## 核心要点

1. Python 子类调用父类方法有 `父类名.方法(self)` 和 `super().方法()` 两种方式，行为不同
2. Python 的"私有"只是名称改写（name mangling），不是真正的访问控制
3. Python 多态是鸭子类型，不需要继承关系也能多态（跟 Java 完全不同）
4. 类属性 vs 对象属性：类属性共享，对象属性独占；修改类属性只能用 `类名.`
5. 类方法 `@classmethod`（参数 cls）、静态方法 `@staticmethod`（无强制参数）的区别
6. `__dict__` 可以把对象转字典，`**dict` 可以把字典解包为构造参数

## 详细笔记

### 子类调用父类方法的两种方式

```python
class Prentice(School, Master):
    def make_master_cake(self):
        # 方式1: 精准调用指定父类
        Master.__init__(self)        # 显式传 self
        Master.make_cake(self)

    def make_old_cake(self):
        # 方式2: 按 MRO 顺序找最近父类
        super().__init__()           # 不需要传 self
        super().make_cake()
```

| | `父类名.方法(self)` | `super().方法()` |
|---|---|---|
| **self** | 必须显式传 | 自动传 |
| **调用目标** | 精准指定某个父类 | 按 MRO 顺序找最近父类 |
| **多继承时** | 想调谁调谁 | 只能按 MRO 链走 |

> 注意：`Master.__init__(self)` 会把 `self.kongfu` 改成 Master 的值，调用完后对象属性已变。这是一个容易忽略的副作用。

### Python 的"私有"只是名称改写

```python
class Prentice:
    def __init__(self):
        self.__money = 20000    # "私有"属性

ts = TuSun()
# ts.__money              # 报错 ❌
# ts._Prentice__money     # 居然能访问 ⚠️
```

**关键区别**：

| | C++/Java private | Python `__attr` |
|---|---|---|
| **能否外部访问** | 绝对不行 | 能！只是改名了：`_ClassName__attr` |
| **本质** | 编译器强制 | 社会约定，不改名就用下划线提醒 |
| **子类能否访问** | 不能 | 不能直接访问，但可以通过改写后的名字访问 |

> 单下划线 `_attr`：约定私有，不改名，外部可访问（纯约定）
> 双下划线 `__attr`：名称改写为 `_ClassName__attr`，外部仍可访问（半强制）
> 真正要限制访问，用 `@property` 装饰器

### Python 多态 = 鸭子类型

课程代码中，`Car` 类没有继承 `Animal`，但 `make_noise(Car实例)` 照样能跑——这在 Java 里不可能（需要实现同一接口）。

```python
# Java 多态: 必须有继承/接口关系
# Python 多态: 只要有同名方法就行（鸭子类型）

class Car:                # 没继承 Animal
    def speak(self):
        print('滴滴滴')

make_noise(Car())         # 照样能跑，只要有 speak() 方法
```

> Python 的多态不依赖继承关系，只要对象有对应的方法就能调用。这就是"鸭子类型"——如果它走起来像鸭子、叫起来像鸭子，那它就是鸭子。

### 类属性 vs 对象属性

```python
class Student:
    teacher_name = '水镜先生'     # 类属性：所有对象共享

    def __init__(self, name):
        self.name = name          # 对象属性：每个对象独有
```

| 操作 | 对象属性 | 类属性 |
|------|---------|--------|
| **访问** | `obj.attr` | `ClassName.attr` 或 `obj.attr`（推荐前者） |
| **修改** | `obj.attr = val`（只改自己） | `ClassName.attr = val`（所有对象都变） |
| **坑点** | — | `obj.attr = val` 不会改类属性，只是给对象新建了一个同名属性！ |

```python
s1.teacher_name = '夯哥'       # ❌ 不会改类属性！只是给 s1 加了个对象属性
Student.teacher_name = '夯哥'  # ✅ 正确改类属性的方式
```

### 类方法 vs 静态方法

```python
class Student:
    school = '黑马程序员'

    @classmethod
    def show_cls(cls):          # cls = 类本身
        print(cls.school)

    @staticmethod
    def show_static():          # 无强制参数
        print(Student.school)
```

| | 类方法 `@classmethod` | 静态方法 `@staticmethod` |
|---|---|---|
| **第一个参数** | `cls`（类对象） | 无强制要求 |
| **能访问类属性** | `cls.属性` | 需要通过 `类名.属性` |
| **使用场景** | 需要操作类本身时（如工厂方法） | 跟类有关但不需要类/实例引用时 |
| **课程案例** | — | `show_view()` 打印菜单，不用 self 也不用 cls |

### `__dict__` 和 `**` 解包

```python
# 对象 → 字典
s1.__dict__          # {'name': '德桦', 'gender': '男', ...}

# [对象] → [字典]
[stu.__dict__ for stu in stu_list]

# 字典 → 对象（两种方式）
Student(d['name'], d['gender'], ...)   # 逐个取值
Student(**d)                            # 解包，更简洁
```

> 保存学生信息时用 `obj.__dict__` 转字典写文件，加载时用 `Student(**dict)` 还原对象。这是整个持久化方案的核心。

### 抽象类（课程版 vs 标准版）

课程中的"抽象类"只是方法体为 `pass` 的普通类，子类不实现也不会报错。Python 标准库 `abc` 提供真正的抽象类：

```python
# 课程版：不强制，子类忘了实现也不报错
class AC:
    def cool_wind(self):
        pass

# 标准版：子类必须实现，否则实例化报错
from abc import ABC, abstractmethod
class AC(ABC):
    @abstractmethod
    def cool_wind(self):
        pass
```

## 关键概念

| 概念 | 解释 |
|------|------|
| 方法重写 | 子类定义与父类同名的方法，调用时优先用子类的（就近原则） |
| `super()` | 按 MRO 顺序调用父类方法，不需要显式传 self |
| 名称改写 | `__attr` → `_ClassName__attr`，Python 的"私有"机制 |
| 鸭子类型 | 不看类型，只看方法是否存在；Python 多态的基础 |
| 类属性 | 定义在类中、方法外，所有对象共享 |
| `@classmethod` | 类方法，第一个参数是 cls（类本身） |
| `@staticmethod` | 静态方法，无强制参数 |
| `__dict__` | 对象的属性字典，用于序列化 |
| `**dict` | 字典解包为关键字参数，用于反序列化 |
| `for...else` | for 循环正常结束走 else，break 跳出则不走（课程学生管理系统中大量使用） |

## 疑问与待深入

- Python `abc` 模块的 `@abstractmethod` 在实际项目中的使用频率？
- `super()` 在多继承 `__init__` 中的协作调用（cooperative multiple inheritance）？

## 与我的关联

- `__dict__` + `**dict` 的序列化模式在配置文件读写中很实用
- 鸭子类型思维在写 Python 时必须刻意练习，C++/Java 的类型思维会拖后腿
