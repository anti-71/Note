---
名称: Numpy 高级用法
章: 04 Python 数据分析
节: "[[第二章 Numpy 运用]]"
tags:
  - 知识点
index: 2
---
##### 数组形状改变

```python
array.reshape(a, b)                  # 改为 a 行 b 列，原数组不变
array.resize(a, b)                   # 改为 a 行 b 列，原数组改变
array.ravel(order='F')               # 降为一维，修改影响原数组
array.flatten(order='F')             # 降为一维，修改不影响原数组
array[np.newaxis, :]                 # 按行增加维度

np.hstack((a1, a2))                  # 横向合并
np.vstack((a1, a2))                  # 纵向合并
np.concatenate((a1, a2), axis=0)     # 合并，0 沿行，1 沿列
np.tile(array, (a, b))              # 沿行复制 a 次，沿列复制 b 次
```

##### 广播机制与通用函数

一维数组按行补齐，二维数组先补列再补行

```python
np.add(a1, a2)                       # 加
np.subtract(a1, a2)                  # 减
np.multiply(array, a)                # 乘
np.divide(array, a)                  # 除
np.power(array, a)                   # 幂
```

##### 集合运算

```python
np.unique(array)                     # 去重
np.intersect1d(a1, a2)              # 交集
np.union1d(a1, a2)                  # 并集
np.setdiff1d(a1, a2)                # 差集
```

##### 搜索与排序

```python
np.sort(array)                       # 升序
np.argsort(array)                    # 返回排序后元素的原始位序
np.argmax(array, axis=0)            # 按行最大元素的位序
np.where(condition, a, b)           # 条件筛选
```

##### NumPy 字符串操作

```python
np.char.upper(array)                 # 转大写
np.char.add(a1, a2)                  # 拼接
np.char.replace(array, s1, s2)      # 替换
np.char.find(array, string)          # 查找
np.char.isdigit(array)               # 是否只含数字
np.char.count(array, string)         # 出现次数
```
