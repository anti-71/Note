---
名称: Matplotlib 高级样式
章: 04 Python 数据分析
节: "[[第五章 数据可视化实战]]"
tags:
  - 知识点
index: 5
---
##### 多子图

```python
plt.subplot(abc)                     # 画布 a 行 b 列，第 c 个子图
plt.subplot2grid(shape, loc, colspan, rowspan)
```

##### 双 y 轴

```python
ax.twinx()
```
