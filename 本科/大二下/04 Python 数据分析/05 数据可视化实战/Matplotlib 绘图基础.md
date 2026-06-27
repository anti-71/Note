---
名称: Matplotlib 绘图基础
章: 04 Python 数据分析
节: "[[第五章 数据可视化实战]]"
tags:
  - 知识点
index: 1
---
##### 基本绘图

```python
plt.plot(x, y, ls=, lw=, c=, marker=, markersize=, label=)
plt.savefig('路径')           # 保存
plt.show()                    # 显示
```

`ls` 折线风格：`'-'` `'--'` `'-.'` `':'`
`marker` 点形状，`lw` 线宽，`c` 颜色，`label` 图例

```python
%matplotlib inline             # 确保图像展示
plt.rcParams['font.sans-serif'] = ['SimHei']  # 中文显示
plt.rcParams['axes.unicode_minus'] = False
```

##### 画布与坐标轴

```python
plt.figure(figsize=(6.4, 4.84), dpi=100, facecolor='white')
plt.title('标题', pad=30, fontsize=12)
plt.xlabel('x', labelpad=20)
plt.ylabel('y', labelpad=20)
plt.xlim([0, 1])               # x 轴范围
plt.ylim([0, 1])               # y 轴范围
plt.xticks([0, 0.2, 0.4])      # x 轴刻度
plt.yticks([0, 0.2, 0.4])      # y 轴刻度
```
