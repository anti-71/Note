# Numpy运用

## 第一节 Numpy基础用法

### 1.数组创建

```python
np.array(list,dtype='')
```

返回一个数组，后面可指定数组内的数据类型

```python
np.arange(a,b,c)
```

创建一个从a到b间隔c的数组

```python
np.linespace(a,b,n)
```

创建一个n个元素，元素值从a到b等差的数组

```python
np.linespace(a,b,n,endpoint=False)
```

产生n+1个等差数列，去除最后一个

```python
np.logspace(a,b,base=c,num=n)
```

创建一个n个元素，各个元素以c为底，指数为a到b的等差数列的数组

```python
np.zeros(a)
```

创建一个全为0的数组，a为数组的形状（二维：[a,b]）

```python
np.ones(a)
```

创建一个全为1的数组，a为数组的形状

```python
np.eye(a)
```

创建一个对角线全为1，其余为0的二位数组，a为数组的形状

```python
np.diag(list) + a
```

创建一个对角线元素由list元素指定的二维数组，其余为0，+a给每个元素+a

```python
array.shape
```

返回数组几行几列

```python
array.ndim
```

返回数组的维度

```python
array.size
```

返回数组的元素个数

```python
array.dtype
```

返回数组的元素类型

### 2.索引和切片

视图上修改，原数据也会发生改变

```python
array_1 = array.copy()
```

这样复制，不会修改原数据

```python
array[a][b]		array[a,b]
```

返回二维数组第a+1行b+1列的元素

```python
array[array > a]
```

返回一个array中大于a的值的数组

```python
array > a
```

返回一个结构与array相同的数组，元素是是否>a的判断

```python
~(array > a)
```

返回上一条相反的结果

```python
array[[a,b]]
```

返回第a+1行和第b+1行的二维数组

```python
array[[a,b],[c,d]]
```

返回第a+1行c+1列和第b+1行d+1列的元素的数组

```python
array[:,[a,b]]
```

返回第a+1，b+1列的二维数组

## 第二节 Numpy高级用法

### 1.数组形状改变

```python
array.reshape(a,b)
```

将原数组改为a行b列的数组，原数组不发生改变

```python
array.reshape(a,-1)
```

将原数组改为a行，列数自动计算的数组

```python
array.reshape(-1)
```

将原数组展开为一维数组，修改其中数据时原数组改变

```python
array.resize(a,b)
```

将原数组改为a行b列的数组，原数组发生改变

```python
array.shape = (a,b)
```

将原数组改为a行b列的数组，原数组发生改变

```python
array.ravel(order = 'F')
```

将数组降为一维（默认横向，此为纵向），修改其中数据时原数组改变

```python
array.flatten(order = 'F')
```

将数组降为一维（默认横向，此为纵向），修改其中数据时原数组不变

```python
array[np.newaxis,:]
```

按行增加一个维度

```python
np.hstack((array_1,array_2))
```

横向合并两个数组，两个数组行数要一致

```python
np.astack((array_1,array_2))
```

纵向合并两个数组两个数组列数要一致

```python
np.concatenate((array_1,array_2),axis=0)
```

合并两个数组，为0沿行，为1沿列

```python
np.tile(array,(a,b))
```

将数组沿行复制a次，沿列复制b次

### 2数组的ufunc广播机制

**广播运算机制**

一维数组，广播运算时，按照行补齐方式，当行数不一致时，首先补齐行数，然后进行运算

二维数组，广播运算时，当列数不一致时，首先补齐列数，然后进行运算，当行数不一致时，首先补齐行数，然后进行运算

**通用函数**

```python
np.add(array_1,array_2)
```

加

```python
np.subtract(array_1,array_2)
```

减

```python
np.multiply(array,a)
```

乘

```python
np.divide(array,a)
```

除

```pyhton
np.power(array,a)
```

幂

```python
np.unique(array)
```

去重

```python
np.in1d(array_1,array_2)
```

返回一个数组，每个元素时array_1的元素是否在array_2中

```python
np.intersect1d(array_1,array_2)
```

交集

```python
np.union1d(array_1,array_2)
```

并集

```python
np.setdiff1d(array_1,array_2)
```

差集

```python
np.equal(array_1,array_2)
```

返回一个数组，每个元素是相同位置的元素是否相等

```python
np.greater(array_1,array_2)
```

返回一个数组，每个元素是相同位置的元素array_1是否大于array_2

```python
array_bool.any()
```

有一个元素为True则返回True

```python
array_bool.all()
```

所有元素为True返回True

```python
np.isnan(array)
```

返回一个数组，每个元素判断是否为nan

### 3.搜索与排序

```python
np.sort(array)
```

将数组升序排序

```python
np.array(sorted(array,reverse = True))
```

将数组降序排序

```python
np.argsort(array)
```

返回一个数组，每个元素为排完序后的元素的原位序

```python
array.argmax()
```

返回数组中最大元素的位序

```python
np.argmax(array,axis=0)
```

返回一个数组，按行最大元素的位序

```python
np.where(condition,a,b)
```

返回一个数组，每个元素按条件判断，满足则为a，不满足则为b（a或b可以不设置）

## 第三节 Numpy文件读写

### 1.数据读取和存储

```python
np.genfromtxt(r'文件路径',delimiter=''，skip_header=a)
```

读取文件，delimiter为分隔符，skip_header跳过表头从第a+1行开始读

```python
np.savetxt(r'路径',array,delimiter='',fmt='%')
```

存储文件，fmt是格式

### 2.字符串操作

```python
np.char.upper()
```

大写，返回数组

```python
np.char.add(array_1,array_2)
```

对相同位置的字符串进行拼接

```pyhton
np.char.multiply(array,a)
```

对字符串法复制a次

```python
np.char.join(array_1,array_2)
```

将array_2中的字符串以array_1中的符号进行逐个字母的拆分

```python
np.char.replace(array,str_1,str_2)
```

将数组中的每个字符串中的str_1换成str_2

```python
np.char.strip(array,a)
```

将数组中字符串中的a符号去除

```python
np.char.find(array,string)
```

返回一个数组，对array中的每个元素字符串匹配，若有，则为第一次出现的位序，若没有，则为-1

```python
np.char.islower(array)
```

返回一个数组，每个元素判断是否只包含小写字母

```python
np.char.isdigit(array)
```

返回一个数组，每个元素判断是否只包含数字

```python
np.char.isalpha(array)
```

返回一个数组，每个元素判断是否只包含字母或汉字

```python
np.char.count(array,string)
```

返回一个数组，每个元素为出现多少次string

```python
np.char.startswith(array,string)
```

返回一个数组，每个元素是否以string开头

```python
np.char.endswith(array,string)
```

返回一个数组，每个元素是否以string结尾

## 第四节 Numpy统计计算

### 1.随机数生成

```python
np.random.seed(n)
```

给一个种子，让随机数随机

```python
np.random.random(n)
```

产生n个0到1之间的浮点数，也可給一个元组生成固定结构

```python
np.random.rand(a,b)
```

产生a行b列均匀分布的0到1的随机数

```python
np.random.radiant(a,b,size=tuple)
```

生成a到b的随机整数，szie为规格

```python
np.random.uniform(a,b,size=tuple)
```

生成a到b的随机数，szie为规格

```python
np.set_printoptions(precision=n)
```

设置小数位数

```python
np.random.normal(a,b,size=tuple)
```

生成正态分布的随机数，a为均值，b为标准差

```python
np.random.radn(a,b)
```

生成标准正态分布

```python
np.random.shuffle(array)
```

对数组内元素随机排序

```python
np.random.permutation(array)
```

对数组内元素随机排序，返回视图

### 2.统计相关函数

```python
array.sum(axis=0)
```

沿行求和

```python
np.sum(array_bool)
```

计算True的数量

```python
array.mean(axis=0)
```

沿行求均值

```python
array.cumsum(axis=0)
```

按行逐个加和

```python
array.cumprod(axis=0)
```

按行逐个求积

```python
array.max(axis=0)
```

按行求最大值

```python
np.percentile(array,a)
```

求数组的a分位数

```python
np.median(array)
```

求数组的中位数

```python
np.ptp(array)
```

求数组的极差

```python
array.ptp(axis=0)
```

按行求极差

### 3.线性代数

```python
np.dot(array_1,array_2)
```

矩阵点乘（相同位置相加最后求和）

```python
np.transpose(array)
```

矩阵转置

```python
np.linalg.inv(array)
```

矩阵求逆

```python
np.diag(array)
```

取出对角线上的元素

```python
np.linalg.solve(A,b)
```

解方程