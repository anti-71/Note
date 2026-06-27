---
名称: Matplotlib 简单图形
章: 04 Python 数据分析
节: "[[第五章 数据可视化实战]]"
tags:
  - 知识点
index: 2
---
##### 饼图

```python
plt.pie(x, explode, labels, colors, autopct='%.1f%%',
        shadow, startangle, radius)
```

##### 条形图

```python
plt.bar(x, y, width, color, bottom, linewidth, tick_label, align)
```

`bottom` 填最底下的值可实现堆叠

##### 直方图

```python
plt.hist(x, bins, range, normed, cumulative, rwidth, color, edgecolor, label)
```

##### 散点图

```python
plt.scatter(x, y, s, c, marker, alpha, linewidths, edgecolors)
```
