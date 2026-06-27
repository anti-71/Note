---
名称: Matplotlib 图形设置
章: 04 Python 数据分析
节: "[[第五章 数据可视化实战]]"
tags:
  - 知识点
index: 3
---
##### 基本设置

```python
plt.gcf()                            # 返回当前图像
fig.set_size_inches(a, b)            # 调整画布大小
plt.grid(ls='--', c='')              # 网格线
plt.axhline(y=, c='', ls='', lw=)    # 水平参考线
plt.axvline(x=, c='', ls='', lw=)    # 垂直参考线
plt.axvspan(xmin=, xmax=, facecolor='', alpha=0.3)  # 覆盖范围
```

##### 图例

```python
plt.legend(loc, bbox_to_anchor, ncol, title, shadow, fancybox)
```

`loc` 位置：`'best'-0` `'upper right'-1` `'upper left'-2` `'lower left'-3`

##### 子图

```python
fig = plt.figure()
ax = fig.add_axes([a, b, c, d])      # 自定义子图位置
plt.subplot(abc)                      # 规则子图
plt.subplot2grid(shape, loc, colspan, rowspan)  # 网格子图
```

##### 注释

```python
plt.text(x, y, string, weight, color)           # 无指向注释
plt.annotate(string, xy=(), xytext=(),
             arrowprops=dict(arrowstyle='->'))   # 指向注释
```
