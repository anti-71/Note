---
名称: Seaborn 高级绘图
章: 04 Python 数据分析
节: "[[第五章 数据可视化实战]]"
tags:
  - 知识点
index: 6
---
##### 基础设置

```python
import seaborn as sns
plt.style.use('seaborn')
sns.set(style='', context='', font_scale=)
```

`style`：`'ticks'` `'darkgrid'` `'white'`

##### 柱状图

```python
sns.barplot(x='Quarter', y='GDP', hue='Industry.Type',
            data=df, color='blue', palette='husl')
```

##### 散点图

```python
sns.scatterplot(x, y, hue, data, palette, style, s, markers)
```

##### 箱线图

```python
sns.boxplot(x, y, hue, data, width, fliersize, linewidth, palette)
```

##### 直方图

```python
sns.displot(data, x, y, hue, bins, hist=True, kde=False)
```

##### 回归图

```python
sns.lmplot(x, y, data, fit_reg=True, height=8, scatter_kws={})
```

##### 计数图

```python
sns.countplot(x, data, hue=)
```
