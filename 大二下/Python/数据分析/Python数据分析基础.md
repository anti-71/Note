# Python数据分析基础

## 第一节 使用入门

### 1.Jupyter notebook快捷操作

*Shift-Enter*：运行本单元，选中下个单元

*Ctrl-Enter*：运行本单元

*Y*：单元转入代码状态

*M*：单元转为markdown状态

*A*：在上方插入单元

*B*：在下方插入单元

*Ctrl-/*：注释整行或撤销注释

## 第二节 数据结构

### 1.**列表**

**定义**	一种不同数据类型元素的有序集合

```python
list.extend(list_1)
```

往列表末尾添加list_1中的元素

```python
list.append(list_1)
```

将list_1作为元素向列表末尾添加

```python
list.pop()
```

默认移出列表末尾元素，可加位序指定删除元素

```python
del list
```

删除整个列表（也可删除指定元素）

```python
list.count(a)
```

返回列表中元素a的个数

```python
list.index(a)
```

返回列表中第一个a的位序

```python
list.insert(index,a)
```

将元素a放入列表的指定位序

```python
list.remove(a)
```

将元素a移出列表（只移出第一个）

```python
list.reverse()
```

将列表反向存放

```python
max(list)
```

返回列表中的最大元素

```python
len(list)
```

返回列表中的元素个数

```python
list_1 + list_2
```

列表支持加法拼接

```python
list * 2
```

列表复制

### 2.**元组**

**定义**	元组是一种有序列表，但元组中的变量不能改变

```python
del tuple
```

删除整个元组

```python
tuple_1 + tuple_2
```

元组支持加法拼接

```python
a,b,c,d = tuple
```

将元组中的值逐一赋值

```python
tuple * 2
```

元组复制

### 3.**集合**

**定义**	集合是一系列无序的、不重复的组合体

```python
set.add(a)
```

添加一个元素a到集合（位序不确定）

```python
set.remove(a)
```

将元素a移出集合

```python
set_1 <= set_2		set_1.issubset(set_2)
```

判断set_1是否为set_2的子集

```python
set_1 - set_2		set_1.difference(set_2)
```

求差集

```python
set_1 | set_2		set_1.union(set_2)
```

求并集

```python
set_1 & set_2		set_1.intersection(set_2)
```

求交集

```python
set_1 ^ set_2		set_1.symmetric_difference(set_2)
```

求并集和交集的差集

### 3.**字典**

**定义**	存放无序的键/值(key/value)映射类型数据的容器

```python
dict.pop(a)		del dict[a]
```

删除字典中键a及其对应的值

```python
a in dict
```

判断键a是否存在于字典

```python
dict.get(a,b)
```

返回字典中键a对应的值，若没有，则返回b（默认为None）

```python
dict.keys()
```

返回字典中的所有键

```python
dict.values()
```

返回字典中的所有值

```python
dict.items()
```

返回字典中的所有键和值

```python
dict.clear()
```

清空字典

## 第三节 函数

### 1.内置函数

```python
round(a,b)
```

将a四舍五入到小数点后b位

```pyhton
type(a)
```

返回a的数据类型

```python
isinstance(a,b)
```

判断a是否为数据类型b

```python
len(a)
```

返回a的长度

```python
for i,j in enumerate(list)
	print(i,j)
```

枚举

```python
zip(list_1,list_2)
```

将相同位置的元素组合成一个元组

### 2.math库函数

```python
math.floor()
```

向下取整

```python
math.ceil()
```

向上取整

### 3.numpy库函数

```python
np.max(list)
```

返回最大值

```python
np.argmax(list)
```

返回最大值的位序



## 第四节 json文件解析

**定义**	Json是JavaScript对象表示法，json格式是一种轻量级的文本数据交换格式，拥有存储空间小，处理速度快等优势

Json本质上是一种嵌套字典格式，但键所对应的值，往往更加复杂，不仅是字，还可以是字符串，数组，列表等

### 1.读取

首先导入库

```python
import json
```

打开文件

```python
with open('文件地址',encoding='utf-8',mode='r') as f:
    f_read = f.read()
```

将文件内容读取成字符串形式

```python
f.readline()
```

一行行读取

```python
f.readlines()
```

以列表读取

```python
data = json.loads(f_read)
```

将字符串解析成json格式

### 2.存储

```python
with open('文件地址\文件名'，'w',encoding='utf-8') as f:
    json.dump(data,f,indent=0)
f.close()
```

## 第五节 字符串处理

### 1.转义字符

*\n*：换行

*\t*：制表符

*\b*：退格

```python
print(r'')
```

不理会字符串里的制表符

### 2.字符串的格式化

```python
print('%s' % a)
```

用a对%s进行替代

s：字符串

d：整数

.nf：n位浮点数

.ne：n位科学计数法

%%：输出%

多个引用使用元组

```python
print('{:%s}'.format(a))
```

等价于上面的形式

### 3.字符串方法

```python
s.split(a)
```

将s以a分割

```python
s.find(a)
```

返回s中匹配到a的第一个位序，若没a，则返回-1

```python
s.lower()
```

以小写形式输出

```python
s.upper()
```

以大写形式输出

```python
s.capitalize()
```

首字母大写

```python
"a".join(list)
```

将list中的所有元素间隔a拼接

```python
s.replace(a,b)
```

将a替换为b

```python
s.strip()
```

去掉首尾空格，lstrip去shoul首，rstrip去尾

```python
s.isdigit()
```

判断字符串是否全由数字组成

```python
s.startswith(a)
```

判断字符串是否由a开头

```python
s.endswith(a)
```

判断字符串是否以a结尾

```python
s.isalnum()
```

判断字符串是否由数字和字母构成

```python
s.count(a)
```

判断a出现几次

## 第六节 高级函数

### 1.lambda函数

```python
f = lambda x:x**2
f(1)
```

匿名函数，可以有若干个参数，参数：想返回的值

### 2.map函数

map()方法会将一个函数映射到序列的每一个元素上，生成新序列，包含所有函数返回值，序列里每一个元素都被当做x变量，到一个函数f(x)里，其结果是f(x1)、f(x2)、f(x3)...组成的新序列

### 3.reduce函数

在选代序列的过程中，首先把前两个元素（只能两个）传给函数，函数加工后，然后把得 修果和第三个元素作为两个参数传给函数参数，函数加工后得到的结果又和第四个元素作为两个参数传给函数参数，依次类推

要导入

```python
from functools import reduce
```

#### 4.filter函数

用于过滤序列，过滤掉不符合条件的元素，返回由符合条件元素组成的序列

## 第七节 数据分析常用库

### 1.Numpy

```python
import numpy as np
```

### 2.Pandas

```python
import pandas as pd
```

### 3.Matplotlib

```python
import matplotlib.pyplot as plt
```

