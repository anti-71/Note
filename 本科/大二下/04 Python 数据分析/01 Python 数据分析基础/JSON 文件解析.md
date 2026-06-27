---
名称: JSON 文件解析
章: 04 Python 数据分析
节: "[[第一章 Python 数据分析基础]]"
tags:
  - 知识点
index: 7
---
##### 定义

JSON 是 JavaScript 对象表示法，是一种轻量级的文本数据交换格式

##### 读取

```python
import json

with open('文件地址', encoding='utf-8', mode='r') as f:
    f_read = f.read()

f.readline()      # 一行行读取
f.readlines()     # 以列表读取

data = json.loads(f_read)   # 将字符串解析成 json 格式
```

##### 存储

```python
with open('文件地址', 'w', encoding='utf-8') as f:
    json.dump(data, f, indent=0)
```
