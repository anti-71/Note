---
名称: Plotly 精美制图
章: 04 Python 数据分析
节: "[[第五章 数据可视化实战]]"
tags:
  - 知识点
index: 7
---
##### 基础

```python
import plotly as py
import plotly.graph_objs as go
from plotly.graph_objs import Scatter
py.offline.init_notebook_mode()
```

##### 散点图

```python
go.Scatter(x, y, mode='markers', name, marker, line)
```

`mode`：`'markers'` 点，`'lines'` 线，`'markers+lines'` 点 + 线

##### 柱状图

```python
go.Bar(x, y, marker=dict(), opacity)
```

##### 直方图

```python
go.Histogram(x, histnorm='probability', marker=dict())
```

##### 饼图

```python
go.Pie(labels, values, hole, textfont=dict())
```

##### 布局控制

```python
go.Layout(title, xaxis, yaxis, legend, barmode='stack')
```

##### 子图

```python
from plotly import tools
fig = tools.make_subplots(rows, cols)
fig.append_trace(trace0, 1, 1)
fig['layout'].update(height, width, title)
py.offline.iplot(fig)
```
