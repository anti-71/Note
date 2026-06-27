---
名称: Numpy 基础用法
章: 04 Python 数据分析
节: "[[第二章 Numpy 运用]]"
tags:
  - 知识点
index: 1
---
##### 数组创建

```python
np.array(list, dtype='')           # 创建数组，可指定数据类型
np.arange(a, b, c)                 # 从 a 到 b 间隔 c 的数组
np.linspace(a, b, n)               # n 个元素，从 a 到 b 等差
np.logspace(a, b, base=c, num=n)   # 以 c 为底，指数 a 到 b 的等比数列
np.zeros(a)                        # 全 0 数组，a 为形状
np.ones(a)                         # 全 1 数组
np.eye(a)                          # 单位矩阵
np.diag(list) + a                  # 对角线为 list，其余为 0
```

##### 数组属性

```python
array.shape    # 行列数
array.ndim     # 维度
array.size     # 元素个数
array.dtype    # 元素类型
```

##### 索引和切片

```python
array.copy()                         # 复制，修改不影响原数据
array[a, b]                          # 第 a+1 行 b+1 列
array[array > a]                     # 返回大于 a 的元素
array > a                            # 返回布尔数组
array[[a, b], [c, d]]               # 返回指定位置元素
array[:, [a, b]]                    # 返回指定列
```
