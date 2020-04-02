---
title: 【Numpy】Numpy入门
tags:
 - python
---

Numpy是开源的python科学计算基础库。

 - 强大的N维对象数组ndarray
 - 广播功能函数
 - 整合C/C++
 - 线性代数、傅里叶、随机数都有生成功能
 
预定俗称： ```import numpy as np```

# 一、N维数组对象：ndarray

ndarray是一个多维数组对象，由两部分构成：
 - 实际的数据
 - 描述这些数据的元数据（数据维度、数据类型等）
 ## 1、ndarray对象的创建
 x = np.array(list/tuple,dtype=np.float32)
 
 即可以用元组或者列表来创建该对象，混用也可以，但数量要一样
 ```py
 a = np.ndarray([[0,1,2,3,4],[9,8,7,6,5]])
```
**创建函数：**
 - **np.arange(n)** 类似range()函数，返回ndarray类型，元素从0到n‐1 
 - **np.ones(shape)** 根据shape生成一个全1数组，shape是元组类型
 - **np.zeros(shape)** 根据shape生成一个全0数组，shape是元组类型 
 - **np.full(shape,val)** 根据shape生成一个数组，每个元素值都是val 
 - **np.eye(n)** 创建一个正方的n*\n单位矩阵，对角线为1，其余为0
 - **np.ones_like(a)** 根据数组a的形状生成一个全1数组 
 -  **np.zeros_like(a)** 根据数组a的形状生成一个全0数组 
 -  **np.full_like(a,val)** 根据数组a的形状生成一个数组，每个元素值都是val
 -  **np.linspace(start,end,Interval,endpoint=true/false)** 根据起止数据等间距地填充数据，形成数组 
 -  **np.concatenate(a,b)** 将两个或多个数组合并成一个新的数组
 
 **维度变换：**
 
 - **.reshape(shape)** 不改变数组元素，返回一个shape形状的数组，原数组不变 
 - **.resize(shape)** 与.reshape()功能一致，但修改原数组 
 - **.swapaxes(ax1,ax2)** 将数组n个维度中两个维度进行调换
 - **.flatten()** 对数组进行降维，返回折叠后的一维数组，原数组不变
 - **.astype()** 方法一定会创建新的数组（原始数据的一个拷贝），即使两个类型一致
 - **.tolist()** 转换成列表

 ## 2、ndarray对象的属性
 -  **.ndim**秩，即轴的数量或维度的数量
 -  .**shapendarray**对象的尺度，对于矩阵，n行m列
 -  **.sizendarray**对象元素的个数，相当于
 -  **.shape**中n\*m的值 （视具体情况而定）
 -  **.dtypendarray**对象的元素类型
 -  **.itemsizendarray**对象中每个元素的大小，以字节为单位

 
 ## 3、ndarray对象的数据类型
 

 -  **bool**布尔类型，True或False 
 -  **intc**与C语言中的int类型一致，一般是int32或int64
 -  **intp**用于索引的整数，与C语言中ssize_t一致，int32或int64 
 -  **int8**字节长度的整数，取值：\[‐128, 127] 
 -  **int16**16位长度的整数，取值：\[‐32768, 32767] 
 -  **int32**32位长度的整数，取值：\[‐231, 231‐1] int6464位长度的整数，取值：\[‐263, 263‐1]
 -  **uint8**8位无符号整数，取值：\[0, 255] 
 -  **uint16**16位无符号整数，取值：\[0, 65535] 
 -  **uint32**32位无符号整数，取值：\[0, 232‐1] 
 -  **uint64**64位无符号整数，取值：\[0, 264‐1] 
 -  **float16**16位半精度浮点数：1位符号位，5位指数，10位尾数
 -  **float32**32位半精度浮点数：1位符号位，8位指数，23位尾数
 -  **float64**64位半精度浮点数：1位符号位，11位指数，52位尾数 
 -  **complex64**复数类型，实部和虚部都是32位浮点数
 -  **complex128**复数类型，实部和虚部都是64位浮点数

 ## 4、ndarray对象的索引和切片
 

 - 索引：获取数组中特定位置元素的过程
 - 切片：获取数组元素子集的过程

一维都类似于对于python中列表的操作。

若取第一维度的第二个维的第3个数据，a\[1,2,3]取，这个和列表的a\[1]\[2]\[3]不同。

切片操作的话：a\[:, 1:3, 1:10:2 ],即取第一维度和第二维度的1-3，第三维度的1，3，5，7，9。
 
  ## 5、ndarray对象的运算
  

 - **a.mean()** 可以得到平均值
 - **a/a.mean()** 即与各元素平均值的商
 - **np.abs(x) np.fabs(x)** 计算数组各元素的绝对值 
 - **np.sqrt(x)** 计算数组各元素的平方根 
 - **np.square(x)** 计算数组各元素的平方 
 - **np.log(x) np.log10(x)  np.log2(x)** 计算数组各元素的自然对数、10底对数和2底对数 
 - **np.ceil(x) np.floor(x)** 计算数组各元素的ceiling值或floor值
 - **np.rint(x)** 计算数组各元素的四舍五入值 
 - **np.modf(x)** 将数组各元素的小数和整数部分以两个独立数组形式返回 
 - **np.cos(x) np.cosh(x) np.sin(x) np.sinh(x) np.tan(x) np.tanh(x)**  计算数组各元素的普通型和双曲型三角函数 
 - **np.exp(x)** 计算数组各元素的指数值 
 - **np.sign(x)** 计算数组各元素的符号值，1(+), 0, ‐1(‐)
 
 
  ## 6、NumPy二元函数
 - **+  ‐ *  /  \*\*** 两个数组各元素进行对应运算 
 -  **np.maximum(x,y) np.fmax() np.minimum(x,y)np.fmin()** 元素级的最大值/最小值计算 
 -  **np.mod(x,y)** 元素级的模运算 
 -  **np.copysign(x,y)** 将数组y中各元素值的符号赋值给数组x对应元素
 -  **> < >= <= == !=** 算术比较，产生布尔型数组


 # 二、数据存取与函数
 ## 1、CSV文件（只能读写一维和二维）
   **写入函数**
 ```py
   np.savetxt(frame, array, fmt='%.18e', delimiter=None)
```
 - frame : 文件、字符串或产生器，可以是.gz或.bz2的压缩文件
 - array : 存入文件的数组
 - fmt: 写入文件的格式，例如：%d %.2f %.18e
 - delimiter : 分割字符串，默认是任何空格

**读出函数**
 ```py
np.loadtxt(frame, dtype=np.float, delimiter=None，unpack=False)
```
 - frame : 文件、字符串或产生器，可以是.gz或.bz2的压缩文件
 - dtype: 数据类型，可选
 - delimiter : 分割字符串，默认是任何空格 
 - unpack : 如果True，读入属性将分别写入不同变量

## 2、多位数据存取（纯二进制文件）
**存入**
 ```py
a.tofile(frame, sep='', format='%s')
```
 - frame : 文件、字符串
 - sep: 数据分割字符串，如果是空串，写入文件为二进制
 
 **读出**
 ```py
np.fromfile(frame, dtype=float, count=‐1, sep='')
```
 - count : 读入元素个数，‐1表示读入整个文件
 - dtype: 读取的数据类型

该方法需要读取时知道存入文件时数组的维度和元素类型 a.tofile()和np.fromfile()需要配合使用 可以通过元数据文件来存储额外信息

## 3、Numpy的便携存取(numpy独特的储存形式,开头有文件信息)
 ```py
np.save(fname, array) #np.savez(fname, array)保存
np.load(fname)#读入
```
 - fname: 文件名，以.npy为扩展名，压缩扩展名为.npz
 - array : 数组变量

## 4、Numpy的随机数函数

 - **rand(d0,d1,..,dn)** 根据d0‐dn创建随机数数组，浮点数，\[0,1)，均匀分布
 - **randn(d0,d1,..,dn)** 根据d0‐dn创建随机数数组，标准正态分布 
 - **randint(low\[,high,shape])** 根据shape创建随机整数或整数数组，范围是\[low, high) 
 - **seed(s)** 随机数种子，s是给定的种子值
 - **shuffle(a)** 根据数组a的第1轴进行随排列，改变数组x 
 - **permutation(a)** 根据数组a的第1轴产生一个新的乱序数组，不改变数组x 
 - **choice(a\[,size,replace,p])** 从一维数组a中以概率p抽取元素，形成size形状新数组 replace表示是否可以重用元素，默认为False。
 - **uniform(low,high,size)** 产生具有均匀分布的数组,low起始值,high结束值,size形状 
 - **normal(loc,scale,size)** 产生具有正态分布的数组,loc均值,scale标准差,size形状 
 - **poisson(lam,size)** 产生具有泊松分布的数组,lam随机事件发生率,size形状

## 5、Numpy的统计函数

 - **sum(a, axis=None)** 根据给定轴axis计算数组a相关元素之和，axis整数或元组
 - **mean(a, axis=None)** 根据给定轴axis计算数组a相关元素的期望，axis整数或元组
 - **average(a,axis=None,weights=None)** 根据给定轴axis计算数组a相关元素的加权平均值
 - **std(a, axis=None)** 根据给定轴axis计算数组a相关元素的标准差
 - **var(a, axis=None)** 根据给定轴axis计算数组a相关元素的方差
 - **min(a)  max(a)** 计算数组a中元素的最小值、最大值
 - **argmin(a)  argmax(a)** 计算数组a中元素最小值、最大值的降一维后下标
 - **unravel_index(index, shape)** 根据shape将一维下标index转换成多维下标
 - **ptp(a)** 计算数组a中元素最大值与最小值的差
 - **median(a)** 计算数组a中元素的中位数（中值）

## 6、Numpy的梯度函数
**np.gradient(f)** 计算数组f中元素的梯度，当f为多维时，返回每个维度梯度
梯度：连续值之间的变化率，即斜率
XY坐标轴连续三个X坐标对应的Y轴值：a, b, c，其中，b的梯度是：(c‐a)/2

 

