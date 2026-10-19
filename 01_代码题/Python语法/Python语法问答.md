# Python 语法问答

> 持续整理 Python 语法问题，形成一份可检索、可复用的速查笔记。

- 分类：Python
- 标签：Python、语法
- 侧边栏：启用

## 背景

- 在当前聊天页面提出 Python 语法问题。
- 所有 Python 语法都追加到本文件，不为每个问题新建 Markdown。
- 个人主页只维护一篇对应文章，内容随本文件持续更新。
- 同类问题优先合并整理，避免重复堆叠。
- 代码示例尽量保持最小可运行，并补充常见误区或适用版本。

## 核心内容

### 问答目录

1. [如何创建 `dict` 和其他常用数据结构](#1-如何创建-dict-和其他常用数据结构)
2. [Python 如何对字符串排序](#2-python-如何对字符串排序)
3. [Python 如何遍历字典](#3-python-如何遍历字典)

### 1. 如何创建 `dict` 和其他常用数据结构

- 日期：2026-10-19

#### `dict`：键值映射

```python
# 空字典
d = {}
d = dict()

# 直接写键值对
user = {"name": "Tom", "age": 18}

# 关键字参数：键只能写成合法的字符串标识符
user = dict(name="Tom", age=18)

# 从 (key, value) 序列创建
d = dict([("a", 1), ("b", 2)])

# 合并两个序列
keys = ["a", "b"]
values = [1, 2]
d = dict(zip(keys, values))

# 字典推导式
squares = {x: x * x for x in range(4)}
```

添加或更新元素：

```python
d["c"] = 3
```

读取键时，`d["missing"]` 会在键不存在时抛出 `KeyError`；不确定键是否存在时可以使用：

```python
value = d.get("missing", 0)
```

#### 其他内置数据结构

| 数据结构 | 空结构 | 带初始值 | 主要用途 |
| --- | --- | --- | --- |
| `list` | `[]` 或 `list()` | `[1, 2, 3]` | 有序、可修改序列 |
| `tuple` | `()` 或 `tuple()` | `(1, 2, 3)` | 有序、不可修改序列 |
| `set` | `set()` | `{1, 2, 3}` | 元素去重、集合运算 |
| `dict` | `{}` 或 `dict()` | `{"a": 1}` | 键值映射 |

两个常见误区：

```python
empty = {}       # dict，不是 set
one_item = (1,)  # 单元素 tuple 必须保留逗号
```

#### 算法题中常见的专用结构

```python
from collections import Counter, defaultdict, deque

queue = deque()            # 双端队列，也常用于 BFS
groups = defaultdict(list) # 访问新键时自动创建空 list
counts = Counter("aab")    # Counter({'a': 2, 'b': 1})

import heapq

heap = []
heapq.heappush(heap, 3)    # Python 的 heapq 默认为小顶堆
smallest = heapq.heappop(heap)
```

选择原则：键值关系用 `dict`，普通可变序列用 `list`，不可变序列用 `tuple`，去重或集合运算用 `set`，先进先出队列用 `deque`。

### 2. Python 如何对字符串排序

- 日期：2026-10-19

#### 按字符排序

`sorted()` 可以接收字符串，但返回的是字符列表：

```python
s = "dcab"
chars = sorted(s)

print(chars)  # ['a', 'b', 'c', 'd']
```

如果需要得到排序后的字符串，使用 `"".join()` 拼接：

```python
s = "dcab"
result = "".join(sorted(s))

print(result)  # abcd
```

降序排列：

```python
result = "".join(sorted(s, reverse=True))

print(result)  # dcba
```

忽略大小写排序：

```python
s = "bAcD"
result = "".join(sorted(s, key=str.lower))

print(result)  # AbcD
```

#### 对字符串列表排序

```python
words = ["pear", "apple", "fig"]

# 按字典序
alphabetical = sorted(words)

# 按字符串长度
by_length = sorted(words, key=len)
```

#### 注意点

- 字符串不可变，也没有原地排序的 `sort()` 方法；`sorted()` 不会修改原字符串，而是创建新列表。
- 默认按字符的 Unicode 顺序比较，因此大写字母、数字和小写字母混合时，结果可能与自然语言顺序不同。
- 对长度为 `n` 的字符串排序，时间复杂度通常为 `O(n log n)`，空间复杂度为 `O(n)`。

### 3. Python 如何遍历字典

- 日期：2026-10-19

假设有如下字典：

```python
user = {"name": "Tom", "age": 18}
```

#### 遍历键

直接遍历字典，默认得到键：

```python
for key in user:
    print(key)
```

也可以显式使用 `keys()`，两种写法效果相同：

```python
for key in user.keys():
    print(key)
```

#### 遍历值

```python
for value in user.values():
    print(value)
```

#### 同时遍历键和值

最常用的是 `items()`：

```python
for key, value in user.items():
    print(key, value)
```

如果还需要序号，可以结合 `enumerate()`：

```python
for index, (key, value) in enumerate(user.items()):
    print(index, key, value)
```

#### 遍历时修改字典

可以修改已有键对应的值，但不要在遍历过程中直接新增或删除键，否则会抛出 `RuntimeError`：

```python
scores = {"Alice": 80, "Bob": 90}

# 可以修改已有值
for name in scores:
    scores[name] += 1

# 删除键时，先遍历键的副本
for name in list(scores):
    if scores[name] < 85:
        del scores[name]
```

Python 3.7 起，字典遍历顺序保证与键的插入顺序一致。

## 示例与实践

下面的例子综合使用字典创建、`get()`、`items()` 和 `sorted()`，统计字符出现次数并按字符排序输出：

```python
text = "banana"
counts = {}

for char in text:
    counts[char] = counts.get(char, 0) + 1

for char, count in sorted(counts.items()):
    print(char, count)
```

输出：

```text
a 3
b 1
n 2
```

## 易错点与边界

- `{}` 创建的是空字典；空集合必须写成 `set()`。
- 单元素元组必须保留逗号，例如 `(1,)`。
- `sorted(string)` 返回字符列表，不是字符串；需要使用 `"".join()` 重新拼接。
- `for x in dictionary` 中的 `x` 是键，不是值。
- 同时需要键和值时使用 `dictionary.items()`，不要再通过键进行一次多余查询。
- 遍历期间不能改变字典的大小；需要新增或删除键时，应遍历副本或先记录待处理的键。

## 总结

选择数据结构时先判断是否需要键值映射、有序可变序列、不可变序列或去重；字符串排序后需要重新拼接；字典则根据需求遍历键、值或键值对。新的 Python 语法问题继续追加到本文件和同一篇主页文章中。

## 参考资料

- [Python 内置函数：`sorted()`](https://docs.python.org/3/library/functions.html#sorted)
- [Python 映射类型：`dict`](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict)
- [Python 教程：循环技巧](https://docs.python.org/3/tutorial/datastructures.html#looping-techniques)
