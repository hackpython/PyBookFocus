# 《利用Python进行数据分析》要点笔记

## 第一章 准备工作

### 1
结构化数据指代所有通用格式数据

+ 多维数组（矩阵）
+ 表格型数据
+ 通过关键列（SQL中的主键和外键）相互联系的多个表
+ 间隔平均或不平均的时间序列

### 2
大部分数据集都可以被转化为更加适合分析和建模的结构化形式，虽然有时这并不明显

也可以将数据集的特征提取为某种结构化形式，如提取新闻中的关键词，进行情感分析

### 3
Numpy(Numerical Python)：python科学计算基础包

+ 快速的多维数组对象ndarray
+ 对数组执行元素级计算和直接对数组执行数学运算函数
+ 用于读写硬盘上**基于数组**的数据集工具
+ 线性代数运算、博里叶变换，随机数生成
+ 基于C、C++、Fortran代码集成到python的工具

### pandas
pandas兼具Numpy高性能的数组计算功能以及电子表格和关系型数据库灵活的数据处理功能

pandas提供大量适用于金融数据的高性能时间序列功能

## 第二章 引言

### 1

使用1.usa.gov数据

?> 2011年，URL缩短服务bit.ly跟美国政府很作，提供一份生成gov或mil短链接的用户匿名数据

2016年服务停止，已经无法从书籍中的链接下载到数据，这里提供一份下载2013年5月17日的旧数据供大家下载，练习书中的代码

[usa.gov数据](https://pan.baidu.com/s/1f0qSNUduSR_QeuYVDu_nNQ)

### 2

使用json库读入数据

```python
In [1]: path = 'usagov_bitly_data2013-05-17-1368832207.txt'

In [2]: import json

In [3]: records = [json.loads(line) for line in open(path)]

In [4]: records[0]
Out[4]:
{'a': 'Mozilla/5.0 (Linux; U; Android 4.1.2; en-us; HTC_PN071 Build/JZO54K) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30',
 'al': 'en-US',
 'c': 'US',
 'cy': 'Anaheim',
 'g': '15r91',
 'gr': 'CA',
 'h': '10OBm3W',
 'hc': 1365701422,
 'hh': 'j.mp',
 'l': 'pontifier',
 'll': [33.816101, -117.979401],
 'nk': 0,
 'r': 'direct',
 't': 1368832205,
 'tz': 'America/Los_Angeles',
 'u': 'http://www.nsa.gov/'}
```
 
### 3
想知道该数据集中最常出现的是哪个时区

```python
In [5]: from collections import defaultdict

# 统计词出现的数量，记录到dict中
In [6]: def get_counts(s):
   ...:     c = defaultdict(int) # 所有值均会初始为0
   ...:     for x in s:
   ...:         c[x] += 1
   ...:     return c
   ...:

# 获得数据中所有的时区
In [7]: time_zones = [rec['tz'] for rec in records if 'tz' in rec]

In [8]: counts = get_counts(time_zones)

In [9]: counts['America/Los_Angeles']
Out[9]: 421

# 获得最大的几个时区，其实就是对记录了时区出现次数的dict进行排序
In [10]: def top_counts(counts_dict, n = 10):
    ...:     v = [(count, tz) for tz, count in counts_dict.items()]
    ...:     v.sort()
    ...:     return v[-n:]
    ...:

In [11]: top_counts(counts)
Out[11]:
[(40, 'America/Phoenix'),
 (50, 'America/Indianapolis'),
 (85, 'Europe/London'),
 (89, 'America/Denver'),
 (102, 'Asia/Tokyo'),
 (184, 'America/Puerto_Rico'),
 (421, 'America/Los_Angeles'),
 (636, ''),
 (686, 'America/Chicago'),
 (903, 'America/New_York')]

# 使用collections.Counter实现上面的需求更加简单
In [12]: from collections import Counter

# Counter方法可以统计list中元素出现的次数
In [13]: counts = Counter(time_zones)

In [14]: counts.most_common(10)
Out[14]:
[('America/New_York', 903),
 ('America/Chicago', 686),
 ('', 636),
 ('America/Los_Angeles', 421),
 ('America/Puerto_Rico', 184),
 ('Asia/Tokyo', 102),
 ('America/Denver', 89),
 ('Europe/London', 85),
 ('America/Indianapolis', 50),
 ('America/Phoenix', 40)]
```

还可以使用pandas对时区进行计数，获得最大的10个时区

```python
In [15]: from pandas import DataFrame, Series

In [16]: import pandas as pd

In [17]: import numpy as np

# records格式：[{},{}..]，读入文件对应的变量
In [18]: frame = DataFrame(records)

In [19]: frame
Out[19]:
       _heartbeat_                                                  a  \
0              NaN  Mozilla/5.0 (Linux; U; Android 4.1.2; en-us; H...
1              NaN  Mozilla/4.0 (compatible; MSIE 7.0; Windows NT ...
2              NaN  Mozilla/5.0 (Windows NT 6.1; rv:21.0) Gecko/20...
3              NaN  Mozilla/5.0 (Linux; U; Android 4.1.2; en-us; S...
.
.
.

In [20]: frame['tz'][:10]
Out[20]:
0     America/Los_Angeles
1
2         America/Phoenix
3         America/Chicago
4
5    America/Indianapolis
6         America/Chicago
7
8           Australia/NSW
9
Name: tz, dtype: object
```

frame输出形式是摘要视图(summary view),用于较大的DataFrame对象

framte['tz']返回的Series对象可以通过value_counts方法获得tz中元素出现次数

```python
In [21]: tz_counts = frame['tz'].value_counts()

In [22]: tz_counts[:10]
Out[22]:
America/New_York        903
America/Chicago         686
                        636
America/Los_Angeles     421
America/Puerto_Rico     184
Asia/Tokyo              102
America/Denver           89
Europe/London            85
America/Indianapolis     50
America/Phoenix          40
Name: tz, dtype: int64
```

用matplotlib为这段数据生成一张图像

先给未知或缺失的时区填上代替值

fillna函数可以替换缺失值(NA)，而未知值（空字符串）可以使用布尔型数组索引替代

```python
In [23]: clean_tz = frame['tz'].fillna('Missing')

In [24]: clean_tz[clean_tz == ''] = 'Unknow'

In [25]: tz_counts = clean_tz.value_counts()

In [26]: tz_counts[:10]
Out[26]:
America/New_York        903
America/Chicago         686
Unknow                  636
America/Los_Angeles     421
America/Puerto_Rico     184
Missing                 120
Asia/Tokyo              102
America/Denver           89
Europe/London            85
America/Indianapolis     50
Name: tz, dtype: int64
```

替换完缺失后，开始绘画，在控制台输入%pylab，让ipython进入pylab模式，进入pylab模式后才能使用matplotlib可视化数据

```python
In [27]: import matplotlib.pyplot as plt

In [28]: %pylab
Using matplotlib backend: TkAgg
Populating the interactive namespace from numpy and matplotlib

In [29]: tz_counts[:10].plot(kind='barh', rot=0)
Out[29]: <matplotlib.axes._subplots.AxesSubplot at 0x114565fd0>
```

![](http://p6un02lk4.bkt.clouddn.com/shuju_fenxi_2-1.png)


