---
名称: Pandas 数据预处理
章: 04 Python 数据分析
节: "[[第三章 Pandas 数据分析应用]]"
tags:
  - 知识点
index: 4
---
##### 时间与类型处理

```python
pd.to_datetime(df[列], format='%Y年%m月', errors='coerce')  # 转标准日期
df[列].astype('float')                                       # 转类型
df[列].dt.year                                                # 提取年份
```

##### 重复值

```python
df.duplicated(subset=列标签, keep='last')   # 判断重复
df.drop_duplicates(subset=列标签, inplace=True)  # 删除重复行
```

##### 缺失值

```python
df.dropna(how='all', axis=0)               # 删除缺失行
df[列].fillna(df[列].median())            # 中位数填补
df.fillna(method='ffill')                 # 前项填补（bfill 后项）
```

##### 异常值

```python
# 均值 ± 1.5 * 标准差
# 上四分位数 + 1.5 * IQR / 下四分位数 - 1.5 * IQR

P1 = df[列].quantile(0.01)
P99 = df[列].quantile(0.99)
df.loc[df[列] > P99, 列] = P99            # 用分位数替换
df.loc[df[列] < P1, 列] = P1
```

##### 数据离散化

```python
pd.cut(df[列], n, labels=range(1, n+1))   # 等宽分段

w = df[列].quantile([i/n for i in range(n+1)])
pd.cut(df[列], w, labels=range(1, n+1))   # 等频分段
```
