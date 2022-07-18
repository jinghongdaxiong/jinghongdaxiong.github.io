---
title: Pandas教程
date: 2022-04-20 17:01:59
tags: [数据处理、python]
categories: [python]
---


# [Pandas简介](https://www.runoob.com/pandas/pandas-tutorial.html)

* Pandas 是 Python 语言的一个扩展程序库，用于数据分析。

* Pandas 是一个开放源码、BSD 许可的库，提供高性能、易于使用的数据结构和数据分析工具。

* Pandas 名字衍生自术语 "panel data"（面板数据）和 "Python data analysis"（Python 数据分析）。

* Pandas 一个强大的分析结构化数据的工具集，基础是 Numpy（提供高性能的矩阵运算）。

* Pandas 可以从各种文件格式比如 CSV、JSON、SQL、Microsoft Excel 导入数据。

* Pandas 可以对各种数据进行运算操作，比如归并、再成形、选择，还有数据清洗和数据加工特征。

* Pandas 广泛应用在学术、金融、统计学等各个数据分析领域。

## Pandas 应用
Pandas 的主要数据结构是 Series （一维数据）与 DataFrame（二维数据），这两种数据结构足以处理金融、统计、社会科学、工程等领域里的大多数典型用例。

## 数据结构

Series 是一种类似于一维数组的对象，它由一组数据（各种Numpy数据类型）以及一组与之相关的数据标签（即索引）组成。

DataFrame 是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数值、字符串、布尔型值）。DataFrame 既有行索引也有列索引，它可以被看做由 Series 组成的字典（共同用一个索引）。

## 相关链接
* Pandas 官网 https://pandas.pydata.org/
* Pandas 源代码：https://github.com/pandas-dev/pandas

# [Pandas 安装](https://www.runoob.com/pandas/pandas-install.html)
安装 pandas 需要基础环境是 Python，开始前我们假定你已经安装了 Python 和 Pip。

使用 pip 安装 pandas:

````
pip install pandas

````

安装成功后，我们就可以导入 pandas 包使用：

````
import pandas
````

实例 - 查看 pandas 版本
````
>>> import pandas
>>> pandas.__version__  # 查看版本
'1.1.5'

````

导入 pandas 一般使用别名 pd 来代替：
````
>>> import pandas
>>> pandas.__version__  # 查看版本
'1.1.5'

````

导入 pandas 一般使用别名 pd 来代替：

````
import pandas as pd
````

实例 - 查看 pandas 版本
````
>>> import pandas as pd
>>> pd.__version__  # 查看版本
'1.1.5'
````

一个简单的 pandas 实例：

````
import pandas as pd

mydataset = {
  'sites': ["Google", "Runoob", "Wiki"],
  'number': [1, 2, 3]
}

myvar = pd.DataFrame(mydataset)

print(myvar)
````

# [Pandas Series](https://www.runoob.com/pandas/pandas-series.html)
Pandas Series 类似表格中的一个列（column），类似于一维数组，可以保存任何数据类型。

Series 由索引（index）和列组成，函数如下：

````
pandas.Series(data, index, dtype, name, copy)
````

**参数说明**
* data：一组数据（ndarray）类型。
* index：数据索引标签，如果不指定，默认从0开始。
* dtype：数据类型，默认会自己判断。
* name：设置名称。
* copy：拷贝数据，默认为False。

创建一个简单的 Series 实例：

````python
import pandas as pd

a = [1, 2, 3]

myvar = pd.Series(a)

print(myvar)

````
输出结果如下

````
0    1
1    2
2    3
dtype: int64

````
从结果可知，如果没有指定索引，索引值就从 0 开始，我们可以根据索引值读取数据：


````python
import pandas as pd

a = [1, 2, 3]

myvar = pd.Series(a)

print(myvar[1])

````
输出结果如下：
````
2
````

我们可以指定索引值，如下实例：

````python
import pandas as pd

a = ["Google", "Runoob", "Wiki"]

myvar = pd.Series(a, index = ["x", "y", "z"])

print(myvar)

````

输出结果如下：
````
x    Google
y    Runoob
z      Wiki
dtype: object

````

根据索引值读取数据:
````
import pandas as pd

a = ["Google", "Runoob", "Wiki"]

myvar = pd.Series(a, index = ["x", "y", "z"])

print(myvar["y"])

````

输出结果如下：
````
Runoob
````

我们也可以使用 key/value 对象，类似字典来创建 Series：

````python
import pandas as pd

sites = {1: "Google", 2: "Runoob", 3: "Wiki"}

myvar = pd.Series(sites)

print(myvar)

````

输出结果如下：

````
1    Google
2    Runoob
3      Wiki
dtype: object

````

从上图可知，字典的 key 变成了索引值。

如果我们只需要字典中的一部分数据，只需要指定需要数据的索引即可，如下实例：
````
import pandas as pd

sites = {1: "Google", 2: "Runoob", 3: "Wiki"}

myvar = pd.Series(sites, index = [1, 2])

print(myvar)
````
输出结果如下：

````
1    Google
2    Runoob
dtype: object

````
设置 Series 名称参数：

````
import pandas as pd

sites = {1: "Google", 2: "Runoob", 3: "Wiki"}

myvar = pd.Series(sites, index = [1, 2], name="RUNOOB-Series-TEST" )

print(myvar)

````
输出结果如下：
````
1    Google
2    Runoob
Name: RUNOOB-Series-TEST, dtype: object

````


# [Pandas DataFrame](https://www.runoob.com/pandas/pandas-dataframe.html)
DataFrame是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数值、字符串、布尔类型）。DataFrame已有行索引也有列索引，它可以被看做由Series组成的字典（共同用一个索引）。

DataFrame构造方法如下：
````
pandas.DataFrame(data, index, columns, dtype, copy)
````

**参数说明**
* data：一组数据（ndarray、series、map、lists、dict等类型）。
* index：索引值，或者可以成为行标签
* columns：列标签，默认为RangeIndex(0,1,2,...,n)。
* dtype：数据类型。
* copy：拷贝数据，默认为False。

Pandas DataFrame是一个二维的数组结构，类似二维数组。

## 使用列表创建
````python
import pandas as pd

data = [['Googele',10],['Runoob',12],['Wiki',13]]

df = pd.DataFrame(data,columns=['Site','Age'],dtype=float)

print(df)

````
输出结果如下：
````
      Site   Age
0  Googele  10.0
1   Runoob  12.0
2     Wiki  13.0

````

## 使用 ndarrays创建

以下实例使用 ndarrays 创建，ndarray 的长度必须相同， 如果传递了 index，则索引的长度应等于数组的长度。如果没有传递索引，则默认情况下，索引将是range(n)，其中n是数组长度。

````python
import pandas as pd

data = {'Site':['Google', 'Runoob', 'Wiki'], 'Age':[10, 12, 13]}

df = pd.DataFrame(Data)

print(df)
````
输出结果如下：

````
0  Google   10
1  Runoob   12
2    Wiki   13

````

## 使用字典创建
还可以使用字典（key/value），其中字典的 key 为列名:
````python
import pandas as pd

data = [{'a': 1, 'b': 2},{'a': 5, 'b': 10, 'c': 20}]

df = pd.DataFrame(data)

print (df)
````
输出结果如下：

````
   a   b     c
0  1   2   NaN
1  5  10  20.0

````
没有对应的部分数据为 NaN。

##  使用 loc 属性返回指定行的数据，如果没有设置索引，第一行索引为 0，第二行索引为 1，以此类推：
````
import pandas as pd

data = {
  "calories": [420, 380, 390],
  "duration": [50, 40, 45]
}

# 数据载入到 DataFrame 对象
df = pd.DataFrame(data)

# 返回第一行
print(df.loc[0])
# 返回第二行
print(df.loc[1])

````
输出结果如下：

````
calories    420
duration     50
Name: 0, dtype: int64
calories    380
duration     40
Name: 1, dtype: int64

````
注意：返回结果其实就是一个 Pandas Series 数据。

## 可以返回多行数据，使用 [[ ... ]] 格式，... 为各行的索引，以逗号隔开：
````
import pandas as pd

data = {
  "calories": [420, 380, 390],
  "duration": [50, 40, 45]
}

# 数据载入到 DataFrame 对象
df = pd.DataFrame(data)

# 返回第一行和第二行
print(df.loc[[0, 1]])
````
输出结果如下：
````
   calories  duration
0       420        50
1       380        40

````
注意：返回结果其实就是一个 Pandas DataFrame 数据。

## 可以指定索引值，如下实例：
````
import pandas as pd

data = {
  "calories": [420, 380, 390],
  "duration": [50, 40, 45]
}

df = pd.DataFrame(data, index = ["day1", "day2", "day3"])

print(df)

````
输出结果如下：

````
      calories  duration
day1       420        50
day2       380        40
day3       390        45

````

## 可以使用 loc 属性返回指定索引对应到某一行：
````
import pandas as pd

data = {
  "calories": [420, 380, 390],
  "duration": [50, 40, 45]
}

df = pd.DataFrame(data, index = ["day1", "day2", "day3"])

# 指定索引
print(df.loc["day2"])

````
输出结果如下：
````
calories    380
duration     40
Name: day2, dtype: int64

````



# [Pandas CSV](https://www.runoob.com/pandas/pandas-csv-file.html)
# [Pandas JSON](https://www.runoob.com/pandas/pandas-json.html)
# [Pandas 数据清洗](https://www.runoob.com/pandas/pandas-cleaning.html)

