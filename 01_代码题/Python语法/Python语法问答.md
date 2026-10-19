# Python 语法问答

这里持续整理 Python 语法问题，作为可检索、可复用的长期笔记。

## 记录方式

- 在当前聊天页面提出 Python 语法问题。
- 每次回答后，将明确、可复用的结论直接追加到本文件，并作为普通 Markdown 文章同步到个人主页和 GitHub。
- 同类问题优先合并整理，避免重复堆叠。
- 代码示例尽量保持最小可运行，并补充常见误区或适用版本。

## 问答目录

1. [如何创建 `dict` 和其他常用数据结构](#1-如何创建-dict-和其他常用数据结构)
2. [Python 如何对字符串排序](#2-python-如何对字符串排序)

## 1. 如何创建 `dict` 和其他常用数据结构

- 日期：2026-10-19

### `dict`：键值映射

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

### 其他内置数据结构

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

### 算法题中常见的专用结构

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

## 2. Python 如何对字符串排序

- 日期：2026-10-19

### 按字符排序

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

### 对字符串列表排序

```python
words = ["pear", "apple", "fig"]

# 按字典序
alphabetical = sorted(words)

# 按字符串长度
by_length = sorted(words, key=len)
```

### 注意点

- 字符串不可变，也没有原地排序的 `sort()` 方法；`sorted()` 不会修改原字符串，而是创建新列表。
- 默认按字符的 Unicode 顺序比较，因此大写字母、数字和小写字母混合时，结果可能与自然语言顺序不同。
- 对长度为 `n` 的字符串排序，时间复杂度通常为 `O(n log n)`，空间复杂度为 `O(n)`。
