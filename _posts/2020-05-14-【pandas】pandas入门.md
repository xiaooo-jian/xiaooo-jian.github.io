---
title: 【pandas】pandas入门
tags:
 - python
---


Pandas 是 Python 的核心数据分析支持库，提供了快速、灵活、明确的数据结构，旨在简单、直观地处理关系型、标记型数据。Pandas 的目标是成为 Python 数据分析实践与实战的必备高级工具，其长远目标是成为最强大、最灵活、可以支持任何语言的开源数据分析工具。经过多年不懈的努力，Pandas 离这个目标已经越来越近了。


Pandas 适用于处理以下类型的数据：

 - 与 SQL 或 Excel 表类似的，含异构列的表格数据;
 - 有序和无序（非固定频率）的时间序列数据;
 - 带行列标签的矩阵数据，包括同构或异构型数据;
 - 任意其它形式的观测、统计数据集, 数据转入 Pandas 数据结构时不必事先标记。



Pandas 的主要数据结构是 **Series（一维数据）** 与 **DataFrame（二维数据**），这两种数据结构足以处理金融、统计、社会科学、工程等领域里的大多数典型用例。对于 R 用户，DataFrame 提供了比 R 语言 data.frame 更丰富的功能。Pandas 基于 NumPy 开发，可以与其它第三方科学计算支持库完美集成。


## 生成对象

用值列表生成 Series 时，Pandas 默认自动生成整数索引
``` python
s = pd.Series([1, 3, 5, np.nan, 6, 8])
```

```python
0    1.0
1    3.0
2    5.0
3    NaN
4    6.0
5    8.0
dtype: float64
```

用 Series 字典对象生成 DataFrame:

``` python
df2 = pd.DataFrame({'A': 1.,
                     'B': pd.Timestamp('20130102'),
                     'C': pd.Series(1, index=list(range(4)), dtype='float32'),
                     'D': np.array([3] * 4, dtype='int32'),
                     'E': pd.Categorical(["test", "train", "test", "train"]),
                     'F': 'foo'})
 
```

``` python
  A          B    C  D      E    F
0  1.0 2013-01-02  1.0  3   test  foo
1  1.0 2013-01-02  1.0  3  train  foo
2  1.0 2013-01-02  1.0  3   test  foo
3  1.0 2013-01-02  1.0  3  train  foo
```
用含日期时间索引与标签的 NumPy 数组生成 DataFrame

``` python
dates = pd.date_range('20130101', periods=6)

DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
               '2013-01-05', '2013-01-06'],
              dtype='datetime64[ns]', freq='D')
			  
df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=list('ABCD'))

   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
2013-01-05 -0.424972  0.567020  0.276232 -1.087401
2013-01-06 -0.673690  0.113648 -1.478427  0.524988
```
## 基本操作

**二进制操作**


DataFrame 支持 add()、sub()、mul()、div() 及 radd()、rsub() 等方法执行二进制操作。广播机制重点关注输入的 Series。通过 axis 关键字，匹配 index 或 columns 即可调用这些函数。


Series 与 Index 还支持 divmod() 内置函数，该函数同时执行向下取整除与模运算，返回两个与左侧类型相同的元组。
``` python
df.shape  # 返回轴的维度
```

**比较操作**


Series 与 DataFrame 还支持 eq（等于）、ne（不等于）、lt（小于）、gt（大于）、le（小于等于）、ge（大于等于） 等二进制比较操作的方法：

``` python
df.gt(df2)

 one    two  three
a  False  False  False
b  False  False  False
c  False  False  False
d  False  False  False
```

**布尔简化**

empty、any()、all()、bool() 可以把数据汇总简化至单个布尔值。


**比较 array 型对象**

用标量值与 Pandas 数据结构对比数据元素非常简单，但如果是对比两个非标量的话就一定要等长的

``` python
pd.Series(['foo', 'bar', 'baz']) == 'foo'

0     True
1    False
2    False
dtype: bool

```

**合并重叠数据集**

合并两个相似的数据集，两个数据集里的其中一个的数据比另一个多。比如，展示特定经济指标的两个数据序列，其中一个是“高质量”指标，另一个是“低质量”指标。如果df1有值，就返回df1的值，如果为nan就使用df2的值

``` python
df1.combine_first(df2)
```

## 数据查看

DataFrame 的列有不同数据类型

``` python
df2.dtypes

A           float64
B    datetime64[ns]
C           float32
D             int32
E          category
F            object
dtype: object
```
查看 DataFrame 头部和尾部数据
``` python
df.head() #查看头部的数据，默认5个
df.tail(3)  #查看尾部的3个数据
```

显示索引与列名：

``` python
df.index
df.columns
```

输出底层数据的 NumPy 对象
``` python
df.to_numpy() #如果df都为浮点数，转换速度就很快，如果多种类型，就消耗较多资源了
						 # 输出不包含行索引和列标签
```

``` python
#快速查看数据的统计摘要：
df.describe()

#转置数据
df.T

#按轴排序
df.sort_index(axis=1, ascending=False)

#按值排序
df.sort_values(by='B')
```

## 选择

获取数据

``` python
#选择单列，产生 Series，与 df.A 等效
df['A']

用 [ ] 切片行，产生df：
df[0:3]


```

按标签选择（理解为坐标系的x，y也是可以的）

``` python
#用标签提取数据
df.loc[:, ['A', 'B']]

#用标签切片，包含行与列结束点
df.loc['20130102':'20130104', ['A', 'B']]

                   A         B
2013-01-02  1.212112 -0.173215
2013-01-03 -0.861849 -2.104569
2013-01-04  0.721555 -0.706771

#提取标量值：
df.loc[dates[0], 'A']
df.at[dates[0], 'A']
```
按位置选择

``` python
#用整数位置选择
df.iloc[3]

#类似 NumPy / Python，用整数列表按位置切片
df.iloc[[1, 2, 4], [0, 2]]


```
布尔索引

``` python
#用单列的值选择数据：
df[df.A > 0]

#选择 DataFrame 里满足条件的值：
 df[df > 0]
 
                    A         B         C         D
2013-01-01  0.469112       NaN       NaN       NaN
2013-01-02  1.212112       NaN  0.119209       NaN
2013-01-03       NaN       NaN       NaN  1.071804
2013-01-04  0.721555       NaN       NaN  0.271860
2013-01-05       NaN  0.567020  0.276232       NaN
2013-01-06       NaN  0.113648       NaN  0.524988


#用 isin() 筛选
df2[df2['E'].isin(['two', 'four'])]
```

## 赋值

``` python
#用索引自动对齐新增列的数据
 s1 = pd.Series([1, 2, 3, 4, 5, 6], index=pd.date_range('20130102', periods=6))

2013-01-02    1
2013-01-03    2
2013-01-04    3
2013-01-05    4
2013-01-06    5
2013-01-07    6
Freq: D, dtype: int64

df['F'] = s1 #然后这个df就得到了上面的值

#按标签赋值：
df.at[dates[0], 'A'] = 0

#按位置赋值
df.iat[0, 1] = 0

#按 NumPy 数组赋值：
df.loc[:, 'D'] = np.array([5] * len(df))


```

## 缺失值

Pandas 主要用 np.nan 表示缺失数据。 计算时，默认不包含空值

``` python
#删除所有含缺失值的行
df1.dropna(how='any')

#填充缺失值：
df1.fillna(value=5)

#提取 nan 值的布尔掩码：
pd.isna(df1)
 A      B      C      D      F      E
2013-01-01  False  False  False  False   True  False
2013-01-02  False  False  False  False  False  False
2013-01-03  False  False  False  False  False   True
2013-01-04  False  False  False  False  False   True
```

## 运算

**统计**


一般情况下，运算时排除缺失值。

``` python
#描述性统计
df.mean()

 s = pd.Series([1, 3, 5, np.nan, 6, 8], index=dates).shift(2)  #shift值向下移动两个，原本没有的就为nan了
 
 2013-01-01    NaN
2013-01-02    NaN
2013-01-03    1.0
2013-01-04    3.0
2013-01-05    5.0
2013-01-06    NaN
Freq: D, dtype: float64

df.sub(s, axis='index')
                   A         B         C    D    F
2013-01-01       NaN       NaN       NaN  NaN  NaN
2013-01-02       NaN       NaN       NaN  NaN  NaN
2013-01-03 -1.861849 -3.104569 -1.494929  4.0  1.0
2013-01-04 -2.278445 -3.706771 -4.039575  2.0  0.0
2013-01-05 -5.424972 -4.432980 -4.723768  0.0 -1.0
2013-01-06       NaN       NaN       NaN  NaN  NaN
```

下表为常用函数汇总表。每个函数都支持 level 参数，仅在数据对象为结构化 Index 时使用


函数|描述|
--|:--:|
count|统计非空值数量|
sum|汇总值|
mean|平均值|
mad|平均绝对偏差|
median	|数中位数|
min|最小值|
max|最大值|
mode|众数|
abs|绝对值|
prod|乘积|
std|贝塞尔校正的样本标准偏差|
var|无偏方差|
sem|平均值的标准误差|
skew|样本偏度 (第三阶)|
kurt|样本峰度 (第四阶)|
quantile|样本分位数 (不同 % 的值)|
cumsum|累加|
cumprod|累乘|
cummax|累积最大值|
cummin|累积最小值|


**最大值与最小值对应的索引**


Series 与 DataFrame 的 idxmax() 与 idxmin() 函数计算最大值与最小值对应的索引， 只返回匹配到的第一个值的 Index

Series 支持 nsmallest() 与 nlargest() 方法，本方法返回 N 个最大或最小的值。对于数据量大的 Series 来说，该方法比先为整个 Series 排序，再调用 head(n) 这种方式的速度要快得多。

s5.mode()可以返回众数


**离散化与分位数**


## 直方图

``` python
s = pd.Series(np.random.randint(0, 7, size=10)) # 随机产生10个 0到7之间的数列

0    3
1    1
2    1
3    3
4    0
5    0
6    1
7    2
8    5
9    4
dtype: int32

s.value_counts() # 计算该值的数量
1    3
3    2
0    2
5    1
4    1
2    1
dtype: int64

```

## 合并

**结合** 
concat用于连接pandas对象
``` python
pieces = [df[:3], df[3:7], df[7:]]
pd.concat(pieces)
```

**连接（join）**

SQL 风格的合并

**追加（Append）**


## 分组

 - 分割：按条件把数据分割成多组；
 - 应用：为每组单独应用函数；
 - 组合：将处理结果组合成一个数据结构。

多列分组后，生成多层索引

``` python
df.groupby(['A', 'B']).sum()  #.sum()可以汇总数据

                  C         D
A   B                        
bar one   -1.814470  2.395985
    three -0.595447  0.166599
    two   -0.392670 -0.136473
foo one   -1.195665 -0.616981
    three  1.928123 -1.623033
    two    2.414034  1.600434
```

## 重塑

**堆叠（Stack）** 


把 DataFrame 列压缩至一层，压缩后的 DataFrame 或 Series 具有多层索引， stack() 的逆操作是 unstack()，默认为拆叠最后一层。


## 数据透视表

``` python
df = pd.DataFrame({'A': ['one', 'one', 'two', 'three'] * 3,
								  'B': ['A', 'B', 'C'] * 4,
								  'C': ['foo', 'foo', 'foo', 'bar', 'bar', 'bar'] * 2,
								  'D': np.random.randn(12),
								  'E': np.random.randn(12)})
								  
  A  B    C         D         E
0     one  A  foo  1.418757 -0.179666
1     one  B  foo -1.879024  1.291836
2     two  C  foo  0.536826 -0.009614
3   three  A  bar  1.006160  0.392149
4     one  B  bar -0.029716  0.264599
5     one  C  bar -1.146178 -0.057409
6     two  A  foo  0.100900 -1.425638
7   three  B  foo -1.035018  1.024098
8     one  C  foo  0.314665 -0.106062
9     one  A  bar -0.773723  1.824375
10    two  B  bar -1.170653  0.595974
11  three  C  bar  0.648740  1.167115

pd.pivot_table(df, values='D', index=['A', 'B'], columns=['C'])

C             bar       foo
A     B                    
one   A -0.773723  1.418757
      B -0.029716 -1.879024
      C -1.146178  0.314665
three A  1.006160       NaN
      B       NaN -1.035018
      C  0.648740       NaN
two   A       NaN  0.100900
      B -1.170653       NaN
      C       NaN  0.536826
```

## 可视化

``` python
ts = pd.Series(np.random.randn(1000),index=pd.date_range('1/1/2000', periods=1000))
ts = ts.cumsum() #计算一个数组各行的累加值
ts.plot()

#折线图
```

## 用字典实现聚合

指定为哪些列应用哪些聚合函数时，需要把包含列名与标量（或标量列表）的字典传递给 DataFrame.agg。


注意：这里输出结果的顺序不是固定的，要想让输出顺序与输入顺序一致，请使用 OrderedDict。

## astype
astype() 方法显式地把一种数据类型转换为另一种，默认操作为复制数据，就算数据类型没有改变也会复制数据，copy=False 改变默认操作模式。