---
名称: Numpy 文件读写
章: 04 Python 数据分析
节: "[[第二章 Numpy 运用]]"
tags:
  - 知识点
index: 3
---
##### 数据读取和存储

```python
np.genfromtxt(r'路径', delimiter='', skip_header=a)  # 读取文件
np.savetxt(r'路径', array, delimiter='', fmt='%')     # 存储文件
```

- `delimiter` 分隔符，`skip_header` 跳过表头，`fmt` 写入格式
