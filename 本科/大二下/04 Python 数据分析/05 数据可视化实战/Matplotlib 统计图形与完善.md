---
名称: Matplotlib 统计图形与完善
章: 04 Python 数据分析
节: "[[第五章 数据可视化实战]]"
tags:
  - 知识点
index: 4
---
##### 箱线图

```python
plt.boxplot(x, notch, sym, vert, whis, widths,
            patch_artist, meanline, showmeans, boxprops={}, labels)
```

##### 时间轴设置

```python
import matplotlib as mpl
ax = plt.gca()
date_format = mpl.dates.DateFormatter('%Y-%m-%d')
ax.xaxis.set_major_formatter(date_format)
xlocator = mpl.ticker.LinearLocator(20)
ax.xaxis.set_major_locator(xlocator)
```

##### 双 y 轴

```python
ax.twinx()                       # 创建共享 x 轴的双 y 轴
```
