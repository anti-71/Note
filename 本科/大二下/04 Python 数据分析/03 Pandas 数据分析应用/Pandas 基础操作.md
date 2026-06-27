---
名称: Pandas 基础操作
章: 04 Python 数据分析
节: "[[第三章 Pandas 数据分析应用]]"
tags:
  - 知识点
index: 1
---
##### Series

由一组数据以及一组与之对应的数据标签（索引）组成

```python
pd.Series(list_1, index=list_2)        # 创建 Series，也可直接给入字典
```

**常用属性**：`values`、`index`、`dtypes`、`shape`、`ndim`、`size`

```python
s.append(Series_2)                     # 拼接
s.drop(string, inplace=True)           # 去除索引和对应数据
```

##### DataFrame

类似数据库中的表或 Excel 表格，既有行索引也有列索引

```python
pd.DataFrame(list_1, columns=list_2, index=list_3)  # 创建 DataFrame
```

**常用属性**：`values`、`index`、`columns`、`dtypes`、`shape`、`ndim`、`size`

##### 数据获取和保存

```python
os.getcwd()                            # 获取当前路径
os.chdir('路径')                        # 切换路径

pd.read_csv('文件', encoding='gbk', dtype={}, nrows=a, na_values=b)
df.head(a)                             # 查看前 a 行
df.tail(a)                             # 查看后 a 行

pd.read_excel('文件', encoding='gbk', sheet_name='表头', dtype={})
df.to_csv('文件.csv', index=False)
df.to_excel('文件.xlsx', index=False)
```

##### 数据筛选

```python
df.loc[A, B]                           # 按行索引和列标签提取
df.iloc[A, B]                          # 按行列位置提取
df[条件][列标签]                        # 直接查询
df[列].between(a, b, inclusive=True)   # 值在 a 到 b 之间
df[列].isin(a)                         # 值为 a
df[列].str.contains(string)            # 包含某字符串
df.drop(列标签, axis=1, inplace=True)   # 删除列
del df[列标签]                          # 删除单列
df.insert(位置, 列标签, 数据)            # 插入列
df.rename(columns={旧: 新}, inplace=True)  # 修改列标签
df.describe(include=['object'])        # 描述统计
```
