# Pandas数据分析应用

## 第一节 Pandas基础操作

 ### 1.常用数据结构

- **Series**

由一组数据以及一组与之对应的数据标签（即索引）组成

```python
pd.Series(list_1，index=list_2)
```

创建一个Series结构，也可直接给入一个字典

​	**常用属性**

values：返回Series对象所有元素

index：返回索引

dtypes：返回数据类型

shape：返回Series数据形状

ndim：返回对象的维度

size：返回对象的个数

用位置索引仍是左闭右开，用索引索引不用左闭右开

```python
Series_1.append(Series_2)
```

Series的拼接

```python
Series.drop(string,inplace=True)
```

去除索引和对应数据，若没有True，则返回视图

- **DataFrame**

DataFrame是pandas基本数据结构，类似数据库中表或者EXCEL中的表格数据格式

DataFrame既有行索引，也有列索引，可以将其看成是多个Series构成的

```python
pd.DateFrame(list_1,columns=list_2,index=list_3)
```

创建一个DataFrame结构，columns是表头

​	**常用属性**

values：返回DataFrame对象所有元素

index：返回索引

dtypes：返回数据类型

shape：返回DataFrame数据形状

ndim：返回对象的维度

size：返回对象的个数

columns：返回列标签

### 2.数据获取和保存

```python
import os
```

操作文件的库

```pyhton
os.getcwd()
```

获取当前路径

```python
os.chdir('文件夹路径')
```

将python文件改到该路径，下面那条就不用输入文件夹路径了，只用输入名称

```python
pd.read_csv('文件名称',encoding='gbk',dtype{'文件内的某一索引':变量类型},nrows=a,na_values=b)
```

读取csv文件，默认读取文件第一行做表头，默认以","为分隔符，gbk编码针对文件内含大量中文，dtype让某一行以：后的变量类型读入，nrows只读取前a行，na_values将b读取为缺失值

```python
df.head(a)
```

查看前a行

```python
df.tail(a)
```

查看后a行

```python
pd.read_excel('文件名称',encoding='gbk',sheet_name='表头',dtype{'文件内的某一索引':变量类型})
```

读取excel文件，参数与read_csv差不多，但一定要设置表头sheet_name

```python
sheet_name = ['表头'+str(i) for i in range(1,a)]
for i in sheet_name:
    data = pd.read.excel('文件名称',encoding='gbk',sheet_name=i,dtype{'文件内的某一索引':变量类型})
    data_all= pd.concat([data_all,data],axis=0,ignore_index=True)
```

批量读取excel文件，并拼接成一个大数据

```python
data.to_csv('文件名称.csv',index=False)
```

保存成csv文件，index不保存

```python
data.to_excel('文件名称.excel',index=False)
```

保存成excel文件

### 3.数据筛选

```python
df.loc[A,B]
```

提取A行B列的数据框，A为行索引，B为列标签，A、B也可以是一个条件判断

```python
df.loc[条件,列标签] = a
```

修改数据

```python
df.iloc[A,B]
```

提取A行B列的数据框，A、B是索引（左闭右开）

```python
df[条件][列标签]
```

直接查询

```python
df[列标签].between(a,b,inclusive=True)
```

用于充当条件，满足某列，值在a到b的行

```python
df[列标签].isin(a)
```

用于充当条件，满足某列，值为a的行

```python
df[列标签].str.contains(string)
```

用于充当条件，满足某列，值包含某个字符串的行

```python
df.drop(列标签,axis=1,inplace=True)
```

删除某列，inplace修改原数据框，否则返回视图

```python
del df[列标签]
```

删除某列，修改原数据框，只能删除单列

```python
df.insert(位置,列标签,数据)
```

插入一列

```python
df.rename(columns={列标签:要改成的列标签},inplace=True)
```

修改列标签（行索引index）

```python
df.describe(include=['object'])
```

描述每行的要素，只描述数据类型是字符串的

## 第二节 Pandas高级操作

### 1.数据库数据读取和保存

```python
import mysql
from sqlalchemy import create_engine
```

导入与sql相关的库和函数

```python
create_engine('mysql+pymysql://user:password@IP:端口号/数据库名')
```

创建一个与数据库的连接

```python
pd.read.sql('sql语句',数据库连接)
```

读取sql

```python
df.to_sql('表名',con=数据库连接,index=False,if_exist='replace')
```

向数据库保存，if_exist如果存在：replace-替换，append-追加，fail-什么也不干

### 2.数据整合

```python
pd.concat([df_1,df_2],axis=1,join='inner')
```

横向连接，inner：交集，只合并有的行索引；outer：并集，没有的行索引后的值填充NaN

```python
pd.concat([df_1,df_2],axis=0,ignore_index=True)
```

纵向连

```python
df.reset_index(drop=True,inplace=True)
```

重置索引

```python
pd.merge(left=df_1,right=df_2,how='right',left_on=列标签,right_on=列标签，sort=True)
```

按左右表列标签下相同的值合并，right：右表全部输出，匹配不上填充NaN；left：左表全部输出，匹配不上填充NaN；inner：只输出匹配上的；outer：全连接，匹配不上填充NaN，sort对连接键的值的大小进行排序

### 3.层次化索引

```python
pd.read_excel(...,index_col=[0,1])
```

用前两列充当行索引

```python
df.loc[a].loc[b]		df.loc[(a,b),:]
```

第0列为a，第1列为b的数据

## 第三节 分组聚合和透视图

### 1.数据排序

```python
np.sum(df.isnull(),axis=0)
```

统计每列上的缺失值个数

```python
df.sort_values(列标签,ascending=True,na_position='last',inplace=True)
```

根据某一列的值进行排序，ascending升序，na_position缺失值放置的位置（first）

### 2.分组聚合

```python
df.groupby(by=列标签)
```

返回一个按某一列的值分组的分组对象

```python
grouped.cumcount()
```

对每个行索引标值，每个组从0开始

```python
grouped.agg([np.mean,np.sum])
```

调用每个分组的多个要素，也可以传入自定义函数

```python
grouped.agg({列标签1:np.sum,列标签2:np.mean})
```

针对不同列调用不同的参数

```python
grouped.apply(np.sum,axis=0)
```

针对每一列使用函数，可以用lambda函数

### 3.透视图和交叉表

```python
pd.pivot_table(data=pd,index,columns,values,aggfunc,fill_value=0,margins=True,margins_name='总计求均值')
```

透视图，Index : 行分组键；columns: 列分组键；values: 分组的字段，只能为数值型变量；aggfunc: 聚合函数；fill_value：缺失值用什么填充margins: 是否需要总计

```python
pd.crosstab(index=df[列标签1],columns=df[列标签2],normalize='all',margins=True)
```

交叉表，返回样本个数，all：计算每个位置数字除以总数；index：计算行的情况；columns：计算列的情况

## 第四节 Pandas数据预处理

### 1.Pandas其他函数运用

```python
pd.to_datatime(df[行标签]，format='%Y年%m月',errors='coerce')
```

将某列转换成标准日期格式，format写当前日期的格式，coerce将不正确的格式返回为空值

```python
df[列标签].astype('float')
```

转化类型

```python
pd.datetime.today()
```

返回现在时间

```python
df[时间的列标签]/np.timedelta64(1,'D')
```

改为几天

```python
df[时间的列标签].dt.year
```

针对时间格式，提取某一个值，如年或月或日的

```python
grouped[列标签].pct_change()
```

计算每个值的变化率

```python
grouped[列标签].rolling(a).mean()
```

计算a行移动平均

```python
grouped[列标签].shift(a)
```

将某列向下平移a行

### 2.数据清洗

**重复值**

```python
df.duplicated(subset=列标签,keep='last')
```

判断是否是重复值，subset为需要判断的列，keep保留何时出现的行，其余重复行为标记为True

```python
df.drop_duplicates(subset=列标签,inplace=True)
```

删除重复行

**缺失值**

```python
df.dropna(how='all',axis=0)
```

按行删除缺失值，all整行都为缺失值删除，any有一个缺失值即删除

```python
df.列标签.fillna(df.列标签.median())
```

用中位数填补缺失值

```python
df.fillna(method='ffill')
```

前项填补，bfill为后项填补

**异常值**

值大于均值 + 1.5 * 标准差的，值小于均值 - 1.5 * 标准差的视为异常值

分位差=上四分位数-下四分位数

值大于均上四分位数 + 1.5 *分位差的，值小于下四分位数 - 1.5 * 分位差的视为异常值

```python
UL= Q3 + 1.5 * IQR
replace_value = sunspots.counts[sunspots.counts < UL].max()
sunspots.loc[sunspots.counts > UL,'counts'] = replace_value
```

用正常值的最大最小值替换

```python
P1 = sunspots.counts.quantile(0.01)
P99 = sunspots.counts.quantile(0.99)
sunspots.loc[ sunspots['counts'] > P99,'counts'] = P99
sunspots.loc[ sunspots['counts'] < P1,'counts'] = P1
```

用九十九分位数和一分位数去替换

### 3.数据离散化

```
pd.cut(df[列标签],n,labels=range(1,n+1))
```

等宽分段

```python
w = [i/n for i in range(n+1)]
pd.qcut(df[列标签],q=w,labels=range(1,n+1))
```

```python
w = df[列标签].quantile([i/n for i in range(n+1)])
pd.cut(df[列标签],w,labels=range(1,n+1))
```

等频分段

