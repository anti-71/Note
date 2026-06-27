---
名称: Pandas 高级操作
章: 04 Python 数据分析
节: "[[第三章 Pandas 数据分析应用]]"
tags:
  - 知识点
index: 2
---
##### 数据库数据读取和保存

```python
from sqlalchemy import create_engine
engine = create_engine('mysql+pymysql://user:password@IP:端口/数据库名')

pd.read_sql('sql语句', engine)           # 读取 SQL
df.to_sql('表名', con=engine, index=False, if_exist='replace')
```

`if_exist`：`replace` 替换，`append` 追加，`fail` 不操作

##### 数据整合

```python
pd.concat([df1, df2], axis=1, join='inner')    # 横向连接
pd.concat([df1, df2], axis=0, ignore_index=True)  # 纵向连接
df.reset_index(drop=True, inplace=True)         # 重置索引
pd.merge(left=df1, right=df2, how='right',
         left_on=列标签, right_on=列标签)        # 按列合并
```

`how`：`inner` 交集、`outer` 全连接、`left`/`right` 保留一侧

##### 层次化索引

```python
pd.read_excel(..., index_col=[0, 1])           # 用前两列做行索引
df.loc[a].loc[b] / df.loc[(a, b), :]           # 多层次索引取值
```
