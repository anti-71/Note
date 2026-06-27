---
名称: Numpy 统计计算
章: 04 Python 数据分析
节: "[[第二章 Numpy 运用]]"
tags:
  - 知识点
index: 4
---
##### 随机数生成

```python
np.random.seed(n)                    # 设置随机种子
np.random.rand(a, b)                 # 均匀分布 [0,1)
np.random.randint(a, b, size=tuple)  # 随机整数
np.random.normal(a, b, size=tuple)   # 正态分布
np.random.shuffle(array)             # 随机排序
```

##### 统计相关函数

```python
array.sum(axis=0)                    # 沿行求和
array.mean(axis=0)                   # 沿行求均值
array.cumsum(axis=0)                 # 按行累加
array.max(axis=0)                    # 按行最大值
np.percentile(array, a)              # 分位数
np.median(array)                     # 中位数
np.ptp(array)                        # 极差
```

##### 线性代数运算

```python
np.dot(a1, a2)                       # 矩阵点乘
np.transpose(array)                  # 矩阵转置
np.linalg.inv(array)                 # 矩阵求逆
np.linalg.solve(A, b)               # 解方程组
```
