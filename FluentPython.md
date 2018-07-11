# 《流畅的python》要点笔记

## 数据模型

### 1
len不是普通方法，而是内置方法，与\__len\__绑定，原因在于快速，list、str等数据结构经常会使用len方法，len快的背后是因为CPython会直接从一个C结构体中读取一个对象的长度，完全不会去调用其他方法，abs也同理

### 2
\__repr\__和\__str\__的区别在于：
\__str\__在str()函数被使用或者print函数打印一个对象时被调用，返回字符串对终端用户更友好
\__repr\__在repr()函数使用时被调用，主要用于debug，要求准确，无歧义，可以表示出对象是如何被构建的

## 数据结构

### 1
容器序列（list、tuple、collections.deque）存放的是他们所包含任何类型的对象的**引用**

扁平序列（str、bytes、bytearray、memoryview、array.array）存放的是值而不是引用

扁平序列内存空间是一段连续的空间，更加紧凑

### 2
python3中列表推导式不再用变量泄露问题

python2.7

```python
In [1]: x = 'ayuliao'

In [2]: d = [x for x in 'abcd']

In [3]: x
Out[3]: 'd'

In [4]: d
Out[4]: ['a', 'b', 'c', 'd']
```

x的值改变，变量泄露

python3.6

```python
In [1]: x = 'ayuliao'

In [2]: d = [x for x in 'abcd']

In [3]: x
Out[3]: 'ayuliao'

In [4]: d
Out[4]: ['a', 'b', 'c', 'd']
```

x的值没有改变

列表推导式只有一个作用，生成列表，而且要短，超过2行，考虑用for循环实现，增加可读性

### 3
生成器表达式遵循迭代器协议，可以逐个产生元素，节省大量内容
语法与列表推导式类似，将[]换成()

### 4
元组除了是不可变列表外，还可以用于没有字段名的记录

元组汇总每个元素都存放了记录中一个字段的数据，外加这个字段的位置，这个位置给数据赋予了意义

### 5
元组拆包：将元组中的元素拆出来给相应的变量

```python
lax_coordinates = (66.666,-118.233)
latitude, _ = lax_coordinates  #元组拆包
print(latitude)
```

元组拆包中，对于不需要使用的元素，可以使用_占位符

### 6
使用*运算符将可迭代对象拆开作为函数参数

```python
In [14]: divmod(20,8)
Out[14]: (2, 4)

In [15]: t = (20,8)

In [16]: divmod(*t)
Out[16]: (2, 4)
```

divmod函数是实现a除以b，然后返回商与余数的元组。如果两个参数a,b都是整数，那么会采用整数除法，结果相当于（a//b, a % b)。如果a或b是浮点数，相当于（math.floor(a/b), a%b)。

### 7
优雅的交换两个变量

```python
a = 233
b = 666
a, b = b, a
```

### 8
python中，\*args来获取**不确定数量**的参数以是一种经典写法

python3中，这个概念扩展到平行赋值中：

```python
In [17]: a,b,*rest = range(5)

In [18]: a,b,rest
Out[18]: (0, 1, [2, 3, 4])

In [19]: a,b,*rest = range(2)

In [20]: a,b,rest
Out[20]: (0, 1, [])

In [21]: a,*rest,c = range(5)

In [22]: a, rest, c
Out[22]: (0, [1, 2, 3], 4)
```

平行赋值中，*前缀只能用在一个变量前，这个变量可以出现在任何位置

### 9
collections.namedtuple：构建带有字段名的元组和一个有名称的类

这个带名称的类对程序调试有很大帮助

collections.namedtuple构建的类的实例消耗的内存与元组一样，字段名都被存到对应的类中，而没有使用\__dict\__来存放这些实例属性，所以这个实例比普通的对象实例占用的内存要小

```python
In [1]: from collections import namedtuple

#一个是类名，一个是类各个字段的名称
In [2]: people = namedtuple('Human','name age birthday position')

In [3]: ayuliao = people('ayu', 23, '02/02',(23.33,66.666))

In [4]: ayuliao
Out[4]: Human(name='ayu', age=23, birthday='02/02', position=(23.33, 66.666))

In [5]: ayuliao.position
Out[5]: (23.33, 66.666)

In [6]: ayuliao[0]
Out[6]: 'ayu'

#_fields属性：包含类所有字段名的元组
In [7]: people._fields
Out[7]: ('name', 'age', 'birthday', 'position')

In [8]: Latlong = namedtuple('Latlong','lat long')

In [9]: d = ('liao', 23, '02/03', Latlong(23.2,52.01314))

In [10]: d
Out[10]: ('liao', 23, '02/03', Latlong(lat=23.2, long=52.01314))

#_make()：接受一个可迭代对象生成一个类的实例，与people(*d)的效果一样
In [11]: dl =  people._make(d)

In [12]: dl
Out[12]: Human(name='liao', age=23, birthday='02/03', position=Latlong(lat=23.2, long=52.01314))

#_asdict()：将具名元组以OrderedDict的形式返回
In [13]: dl._asdict()
Out[13]:
OrderedDict([('name', 'liao'),
             ('age', 23),
             ('birthday', '02/03'),
             ('position', Latlong(lat=23.2, long=52.01314))])
```

### 10
元组中没有\__reversed\__方法，该方法只有一个优化而已，reversed()方法在没有\__reversed\__的情况下也可以使用

s.reversed():将s中的元素倒序排列（从大到小）

### 11
切片和区间都会忽略最后一个元素，这是因为python、c等语言都是以0作为起始下标

+ range(3)与my_list[:3]都返回3个元素
+ 可以快速计算切片和区间的长度，下标（stop-start)即可
+ 分割成不重叠的两部分，my_list[:3]与my_list[3:]则可

### 12
对seq[start:stop:step]求值时，python会调用seq.\__getitem\__(slice(start,stop,step))方法

```python
In [22]: l = [x for x in range(10)]

In [23]: l
Out[23]: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

In [24]: l[2:8:2]
Out[24]: [2, 4, 6]
```

### 13
[]运算符可以通过逗号分开多个索引或切片，外部看Numpy就使用了这个特性

二维的numpy.ndarray可以通过a[i,j]获得值或a[m:n,k:l]获得切片，要处理这种运算，\__getitem\__和\__setitem\__需要以元组的方式来接受a[i,j]中的索引

python内置的序列类型都是一维的，只支持单一的索引

### 14
省略符：三个英文句号...在python解释器眼里是有特殊意义的，...实际上Ellipsis对象的别名，而Ellipsis对象优势ellipsis类的单一实例，它可以当做切片的一部分，如l[a,...,z]

在Numpy中...用作多维数组切片的快捷方式，如x是四维数组，x[i,:,:,:]可以缩写成x[i,...]

### 15
将切片放在赋值语句的左边或作为del操作的对象，就可以对序列进行嫁接、切除或就地修改

```python
In [25]: l = list(range(10))

In [26]: l
Out[26]: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

#删除0到2的数
In [27]: del l[0:3]

In [28]: l
Out[28]: [3, 4, 5, 6, 7, 8, 9]

#替换7到9的数，没有就链接上去
In [29]: l[7:10] = [6,66]

In [30]: l
Out[30]: [3, 4, 5, 6, 7, 8, 9, 6, 66]

#替换1到2的数
In [31]: l[1:3] = [11,12,13]

In [32]: l
Out[32]: [3, 11, 12, 13, 6, 7, 8, 9, 6, 66]

#切片赋值，右侧必须是可迭代对象
In [33]: l[0:3] = 100
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-33-4bcb15f3904f> in <module>()
----> 1 l[0:3] = 100

TypeError: can only assign an iterable

In [34]: l[0:3] = [100]

In [35]: l
Out[35]: [100, 13, 6, 7, 8, 9, 6, 66]
```

### 16
+和*都不会修改原有的操作对象，而是构建一个全新的序列

```python
In [36]: l = list(range(3))

In [37]: l*3
Out[37]: [0, 1, 2, 0, 1, 2, 0, 1, 2]

In [38]: l
Out[38]: [0, 1, 2]
```


### 17
如果用my_list[[]] * 3初始会一个由列表组成的列表是，你得到的列表中包含的3个元素其实是3个引用，它们指向同一个列表

通过下面的代码理解上面的话

```python
# a中的list不是同一个引用
In [39]: a = [['_'] * 3 for i in range(3)]

In [40]: a
Out[40]: [['_', '_', '_'], ['_', '_', '_'], ['_', '_', '_']]

In [41]: a[1][2] = 'A'

In [42]: a
Out[42]: [['_', '_', '_'], ['_', '_', 'A'], ['_', '_', '_']]

# b中的list是同一个引用，乘号的作用是复制list中的内容，这就导致list中的引用是一样的
In [43]: b = [['_'] * 3]*3

In [44]: b
Out[44]: [['_', '_', '_'], ['_', '_', '_'], ['_', '_', '_']]

In [46]: b[1][2] = 'B'

In [47]: b
Out[47]: [['_', '_', 'B'], ['_', '_', 'B'], ['_', '_', 'B']]
```

### 18
+=、*=表现取决于它们第一个操作对象

如果+=第一个操作对象是可变对象，那么+=背后的特殊方法就是\_iadd\__方法，就地改动，不会创建新对象

如果+=第一个操作对象是不可变对象，那么+=背后的特殊方法就是\__add\__方法，创建新对象，将获得的值赋值给创建的新对象

```python
n [13]: l = [5,2,0]

In [14]: id(l)
Out[14]: 4383654728

In [15]: l *= 3

In [16]: l
Out[16]: [5, 2, 0, 5, 2, 0, 5, 2, 0]

# l是可变对象，
In [17]: id(l)
Out[17]: 4383654728

In [18]: t = (5,2,0)

In [19]: id(t)
Out[19]: 4382327432

In [20]: t *= 3

In [21]: t
Out[21]: (5, 2, 0, 5, 2, 0, 5, 2, 0)

In [22]: id(t)
Out[22]: 4383320264
```

### 19
一个有趣的现象

```python
In [24]: t = (1,2,[3,4])

In [25]: t[2] += [5,6]
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-25-c823147bfbc0> in <module>()
----> 1 t[2] += [5,6]

TypeError: 'tuple' object does not support item assignment

In [26]: t
Out[26]: (1, 2, [3, 4, 5, 6])
```

t[2]改动成功，但也有异常抛出，可以通过python可视化工具看看代码发生了什么

<iframe width="800" height="200" frameborder="0" src="http://pythontutor.com/iframe-embed.html#code=t%20%3D%20%281,2,%5B3,4%5D%29%0At%5B2%5D%20%2B%3D%20%5B5,6%5D&codeDivHeight=400&codeDivWidth=350&cumulative=false&curInstr=2&heapPrimitives=nevernest&origin=opt-frontend.js&py=3&rawInputLstJSON=%5B%5D&textReferences=false"> </iframe>

造成虽然报错，但依旧会改动成功现象的原因如下：

+ 1.tuple不支持对它的元素赋值，会抛出TypeError异常
+ 2.+=或*=不是原子操作，其背后由多条字节代码做支撑
+ 3.在多条字节码中，先做了将值添加的操作，因为t[2]对应是可变对象，所以添加成功，然后再做赋值操作，将变动后的值赋值给t，因为t是tuple，所以抛出异常，但在抛出异常前，可变对象中已经存在最新的数据了

### 20
list.sort方法就地排序列表，不会把原列表复制一份，返回值为None

内置函数sorted，会新建列表作为返回值，该方法可以借助任何信息的可迭代对象作为参数，包括不可变序列和生成器

list.sort和sorted都有两个可选参数，reverse参数和key参数

+ reverse：为True，序列以降序排序，False为升序排序，默认为False
+ key：只能穿一个参数的函数，函数会被用在序列里的每一个元素，所产生的结果将是排序算法依赖的对比关键字，如key=str.lower实现忽略大小写的排序，或key=len基于字符串长度排序

Python中无论是list.sort还是sorted使用的排序算法都是Timsort，这种排序算法是稳定的，也就是在排序时，如果两者对比大小相同时，相对位置不会改变

```python
In [27]: f = ['ab','acd','eeee','cc','wew','werwa']

In [28]: sorted(f)
Out[28]: ['ab', 'acd', 'cc', 'eeee', 'werwa', 'wew']

In [30]: sorted(f,key=len)
Out[30]: ['ab', 'cc', 'acd', 'wew', 'eeee', 'werwa']

# acd与wew的相对位置没有改变
In [32]: sorted(f,key=len,reverse=True)
Out[32]: ['werwa', 'eeee', 'acd', 'wew', 'ab', 'cc']
```

list.sort、sorted、max、min函数的key参数都是一个很棒的设计。

其他语言里，排序函数一般需要用户提供了两个参数用于比较，如Python2中的cmp(a,b)，但key只需要一个参数

而且key函数更加高效，因为在每个元素上，key函数只会被调用一次，而双参数比较函数则会在每一次两两比较的时候都会被调用。虽然在排序时，python终究会比较两个key，但是那一阶段的计算发生在C语言层面，这会比滴啊用用户自定义的函数更快

### 21
bisect模块主要包含两个函数，bisect和insort，两个函数都利用二分查找算法在有序序列中查找或插入元素

bisect(haystack, needle):在haystack中搜索needle的位置，该位置满足把needle插入这个位置后，haystack依旧保持升序，也就是这个位置前的值都<=needle的值，**其中haystack必须是一个有序的序列**

可以先用bisect(haystack, needle)查找位置index，在用haystack.insert(index, needle)来插入新值，但也可以使用insort一步到位

```python
In [15]: HAYSTACK = [1,4,7,9,12,16,20,30]

In [16]: bisect.insort(HAYSTACK,8)

# 依旧升序
In [17]: HAYSTACK
Out[17]: [1, 4, 7, 8, 9, 12, 16, 20, 30]
```


### 22
用bisect建立一个用数字作为索引的查询表格，如将分数和成绩匹配

```python
In [21]: def g(s, number= [60,70,80,90], grades = 'FDCBA'):
    ...:     i = bisect.bisect(number, s)
    ...:     return grades[i]
    ...:

In [22]: [g(i) for i in [33,55,88,99,77,100]]
Out[22]: ['F', 'F', 'B', 'A', 'C', 'A']
```

bisect可以对几乎所有的序列类型进行查找和插入操作

### 23
列表不用滥用，有时会有更好的选择

如果要存放1000万个浮点数，使用数组(array)效率要高很多，因为数组背后存的并不是float对象，而是数字的机器码，也就是字节标书

如果要频繁对序列做先进先出的操作，使用双端队列(deque)速度会快很多

### 24
如果要一个只包含数字的列表，array.array比list更高效

数组支持所有可变序列有关操作，包括pop,insert,extend，因为数组提供了frombytes和tofile，可以快速的读取和存入文件

创建数组需要类型码，该类型码用来表示在底层的C语言应该存放怎么样的数据，如b类型码表示有符号的字符，因此array('b')创建出的数组就只能存放一个字节大小的整数，范围从-128到127，在序列很大的时候，能节省很多空间，**而且Python不会允许你在数组里存放除指定类型之外的数据**

```python
In [23]: from array import array

In [24]: from random import random

# 创建大小为1000万的数组，速度很快
In [25]: f = array('d', (random() for i in range(10**7)))

In [27]: f[-1]
Out[27]: 0.2964141610632067

In [28]: fp = open('/Users/ayuliao/Desktop/f.bin','wb')

In [29]: f.tofile(fp)

In [30]: fp.close()

In [31]: f2 = array('d')

In [32]: fp = open('/Users/ayuliao/Desktop/f.bin','rb')

# 从文件中读取1000万个数
In [34]: f2.fromfile(fp,10**7)

In [35]: fp.close()

In [36]: f2[-1]
Out[36]: 0.2964141610632067

In [37]: f == f2
Out[37]: True
```

使用array.fromfile从一个二进制文件里读出1000万个双精度浮点数只需要0.1秒，比从文本中读取要快60倍，因为从文本中读取时，会使用内置的float方法把每一行文字都转换成浮点数

使用array.tofile写入到二进制文件，比以每行一个浮点数的方式写入文本文件要快7倍

除了使用array中处理文件的方法，还可以使用pickle模块，pickle.dump处理浮点数组的速度几乎跟array.tofile一样快，但pickle.dump可以处理几乎所有的内置数据，甚至是用户自定义的类（类没有复杂的实现）

python3.4开始，数组类型不支持list.sort()这种就地排序方法，要个数组排序，要使用sorted()函数，新建一个数组，bisect.insort方法在数组中依旧有效

### 25
memoryview(内存视图)是个内置类，让用户在不复制内容的情况下操作同一个数组的不同切片

> 内存视图其实是泛化和去数字化的Numpy数组，可以让你在不需要复制内容的前提下，对数据机构之间共享内存，这种功能在处理大型数据集合适非常重要

memoryview.cast 的概念跟数组模块类似，能用不同的方式读写同一块内存数据。memoryview.cast会把同一块内存里的内容打包成一个全新的memoryview对象给你

### 26
Numpy二维数组基本操作

```python
In [1]: import numpy as np

In [2]: a = np.arange(12)

In [3]: a
Out[3]: array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11])

In [4]: type(a)
Out[4]: numpy.ndarray

In [5]: a.shape
Out[5]: (12,)

#改变形状
In [6]: a.shape = 3,4

In [7]: a
Out[7]:
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])

In [8]: a[2]
Out[8]: array([ 8,  9, 10, 11])

In [9]: a[2,1]
Out[9]: 9

In [10]: a[:,1]
Out[10]: array([1, 5, 9])

# 转置
In [11]: a.transpose()
Out[11]:
array([[ 0,  4,  8],
       [ 1,  5,  9],
       [ 2,  6, 10],
       [ 3,  7, 11]])
```

### 27
numpy.load()方法利用内存映射机制，它可以在内存不足的情况下仍然可以对数组做切片

Python比较强大的科学计算库：Numpy、Scipy、Pandas、Blaze

### 28
collections.deque类(双向队列)是一个线程安全、可以快速从两端添加或删除元素的数据类型

如果要有一种数据类型来存放“最近用到的几个元素”，deque是个很好的选择

```python
In [16]: from collections import deque

# maxlen可选参数，设定队列可容纳元素的数量，一个设定，不可修改
In [17]: dq = deque(range(10), maxlen=10)

In [18]: dq
Out[18]: deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

# rotate选择操作，n>0，队列最右边n个元素被移动到左边
In [19]: dq.rotate(3)

In [20]: dq
Out[20]: deque([7, 8, 9, 0, 1, 2, 3, 4, 5, 6])

# n<0，队列最左边n个元素被移动到右边
In [21]: dq.rotate(-4)

In [22]: dq
Out[22]: deque([1, 2, 3, 4, 5, 6, 7, 8, 9, 0])

# 向队列中添加元素，appendleft从左往右添加，队列满时，最右边的元素出队
In [23]: dq.appendleft(-1)

In [24]: dq
Out[24]: deque([-1, 1, 2, 3, 4, 5, 6, 7, 8, 9])

In [25]: dq.append(1)

In [26]: dq
Out[26]: deque([1, 2, 3, 4, 5, 6, 7, 8, 9, 1])

# extend方法添加的必须是iter
In [27]: dq.extend([11,22,33])

In [28]: dq
Out[28]: deque([4, 5, 6, 7, 8, 9, 1, 11, 22, 33])
```

### 29
除了deque，其他python标准库也提供了队列的实现

queue库：提供了同步、线程安全的Queue、LifoQueue和PriorityQueue，不同的线程可以利用这些数据类型交互信息。他们都有maxsize这个可选参数，用于限定队列大小，如果满员时，这些类不会扔掉旧的元素，而是加锁，知道另外的线程移除某个元素腾出位置。这一特性很适合控制活跃线程的数量

multiprocessing库：实现了自己的Queue，与queue.Queue类似，只是设计给进程间通信使用的，还有个multiprocessing.JoinableQueue类型，可以让任何管理更加方便

asyncio库：python3.4新添加的标准库，里面有Queue、LifoQueue、PriorityQueue和JoinableQueue，为异步编程提供便利

heapq库：heapq没有队列类，而是提供了heappush和heappop方法，可以将可变序列当作堆队列或优先队列来使用

### 30
Timbot算法

sorted和list.sort背后的排序算法都是Timbot，Timbot是一种自适应算法，会根据原始数据的顺序特点交替使用插入排序和归并排序，以达到最佳效率

Timbot算法是很高效的算法，因为来自真实世界的数据通常都是有一定顺序特点的





## 字典和集合

### 1

dict类型是python语言中的基石，模块命名空间、实例的属性和函数的关键字参数中都可以看到字典的身影。

与dict有关的内置喊都在 `__builtins__.__dict__`模块中

因为字典非常重要，python对它做了高度优化，**散列表则是字典类型性能出众的根本原因**

集合(set)的实现也依赖于散列表

### 2

`collections.abc`模块中有Mapping和mutableMapping这个两个抽象基类，作用就是为dict何其他类似的类型定义形式接口

非抽象映射类型一般不会直接继承这些抽象基类，直接对dict或collections.User.Dict进行扩展，这些抽象基类主要作用是作为形式化的文档，定义了构建一个映射类型所需要的最基本的接口。它们还可以跟isinstance一起用来判断某个数据是不是广义上的映射类型。

```python
In [5]: from collections import abc

In [6]: d = {}

In [7]: isinstance(d, abc.Mapping)
Out[7]: True
```

### 3
python标准库里的所有映射类型都是利用dict来实现的，因此它们都有个共同的限制，即**只有可散列的数据类型**才能用作这些映射力的键，只有键有这个要求，值并不需要是可散列的数据类型。

?>可散列：如果一个对象是可散列的，那么这个对象的生命周期中，**它的散列值是不变的**，而且这个对象需要使用`__hash__()`方法。另外可散列还要有`__qe__()`方法，这样才能更其他键做比较，如果两个可散列对象是相等的，那么它们的散列值一定是一样的

**原子不可变的数据类型（如：str、buytes和数值类型）都是可散列类型**，frozenset也是可散列的，根据frozenset定义，frozenset力只能容纳可散列类型。

对于元组而言，只有当元组包含的所有元素都是可散列类型的情况下，它才是可散列的

```python
In [9]: t = (1,2,(3,4))

In [10]: hash(t)
Out[10]: -2725224101759650258

In [11]: t1 = (1,2,[3,4]) #元组中引用了可变变量list，那么就无法进行散列了

In [12]: hash(t1)
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-12-4375c565e98a> in <module>()
----> 1 hash(t1)

TypeError: unhashable type: 'list'

In [13]: tf = (1,2,frozenset([30,40]))

In [14]: hash(tf)
Out[14]: -4118419923444501110
```

### 4
一般而言，用户自定义的类型的对象都是可散列的，散列值就是它们的id()函数的返回值，所以所有这些对象在比较的时候都是不相等的。（散列值不同）

如果一个对象实现了`__eq__`方法，并且在方法中用到了这些对象的内部状态，那么只有当所有这些内部状态都是不可变的情况下，这个对象才是可散列的。

字典提供了多种定义方式

```python
In [24]: a = dict(a=1,b=2)

In [25]: b = {'a':1,'b':2}

In [26]: c = dict(zip(['a','b'],[1,2]))

In [27]: d = dict([('a',1),('b',2)])

In [28]: e = dict({'a':1,'b':2})

In [29]: a == b == c == d == e
Out[29]: True
```

### 5

字典推到可以从任何以键值对作为元素的可迭代对象中构建出字典

```python
In [30]: m = [('a',1),('b',2)]

In [31]: dict(m)
Out[31]: {'a': 1, 'b': 2}

In [32]: c = {k:v for k,v in m} #字典推到式

In [33]: c
Out[33]: {'a': 1, 'b': 2}
```

### 6

```
d.update(m, [**kwargs])
```

update方法处理参数m的方式，是典型的**鸭子类型**，函数首先先检查m是否有keys方法，如果有，那么update函数就把它当作映射对象来处理，否则，函数把m当作包含了键值对（key,value)元素的迭代器

python中大多数映射类型的构造方法都采用了类型的逻辑，因此你可以用一个映射对象来新建一个映射对象，也可以用包含（key,value)元素的可迭代对象来初始化一个映射对象。

### 7

当字典`d[k]`找不到正确的键时，python就会报错，Python信奉“快速失败”哲学。

可能大家都知道使用`d.get(k,default)`来代替`d[k]`，给找不到的键一个默认的返回值，但要**更新某个键对应的值的时候**，不管使用`__getitem__`还是`get`都会不自然，而且效率低。`dict.get`并不是处理找不到的键的最好方法。

使用<!-- more -->方法最佳

```python
my_dict.setdefault(key, []).append(new_value)
```

等价于

```python
if key not in mydict:
    mydict[key] = []
mydict[key].append(new_value)
```

`mydict[key].append(new_value)`这句代码，将新的内容放回到字典中，牵扯到了一次查询，所以第二种写法效率明显没有第一种高

两者效果一样，只是后者至少要进行两次键查询，如果键不存在，就是进行三次查询，使用setdefault只需要一次就可以完成整个操作了

直观的体会一下，下面编写一段 从索引中获取单词出现的频率信息，并把他们写进对应的列表里

```python
import sys
import re

WORD_RE = re.compile(r'\w+')

index = {}

with open(sys.argv[1], encoding='utf-8') as fp:
    for line_no, line in enumerate(fp, 1):
        for match in WORD_RE.finditer(line):
            word = match.group()
            column_no = match.start() + 1
            # 这种实现方法不好
            location = (line_no, column_no) 
            occ = index.get(word, []) #1
            occ.append(location) #2
            index[word] = occ #3
            
for word in sorted(index, key=str.upper): #4
    print(word, index[word])
```

+ 1.提取word出现的情况，如果没有记录，就返回[]
+ 2.把新单词出现位置添加到列表后面
+ 3.把新的列表放回字典中，**牵扯了一次查询操作**，先找到在哪里，才能更新
+ **4.sorted函数的`key=`参数没有调用`str.upper`，而是把这个方法的引用传递给了sorted函数，这样的排序的时候，单词就好被规范成统一的格式**

使用`setdefault`来改进一下

```python
with open(sys.argv[1], encoding='utf-8') as fp:
    for line_no, line in enumerate(fp, 1):
        for match in WORD_RE.finditer(line):
            word = match.group()
            column_no = match.start() + 1
            # 这种实现方法不好
            location = (line_no, column_no)
            # occ = index.get(word, [])
            # occ.append(location)
            # index[word] = occ
            index.setdefault(word,[]).append(location) #1
```

+ 1.先获取word这个key下的value，也就是获取单纯出现情况列表，如果单词不存在，就把单词和一个空列表放入映射













## 函数装饰器和闭包

### 1
函数装饰器用于在源码中“标记”函数，以某种方式增强函数的行为，想要掌握装饰器，必须先理解闭包

nonlocal是新出现的保留关键字，在python3.0引入，如果自己想实现函数装饰器，就必须了解闭包，因此也需要知道nonlocal

### 2
装饰器是可调用的对象，其参数是另一个函数，python支持类装饰器。装饰器可能会处理被装饰的函数，然后把它返回，或将其替换成另一个函数或可调用对象

简单例子理解上面这句话

```python
In [1]: def deco(func):
   ...:     def func2():
   ...:         print('Hello world')
   ...:     return func2
   ...:
   ...: @deco
   ...: def func1():
   ...:     print('Hello ayuliao')
   ...:

In [2]: func1()
Hello world

In [3]: func1
Out[3]: <function __main__.deco.<locals>.func2>
```

func1函数使用了deco装饰器，看到Out[3]，可以发现func1函数其实被装饰器中的func2函数函数替换了

装饰器的一大特性就是被装饰的函数替换成其他函数，第二大特性就是装饰器在加载模块时立即执行

### 3

装饰器一个关键特性是，它们在被装饰的函数定义之后立即执行，比如import导入时

```python
funclist = []

def reg(func):
    print('running func is (%s)'%func)
    funclist.append(func)
    func()
    return func

@reg
def f1():
    print('running f1()')
    
@reg
def f2():
    print('running f2()')
    
def f3():
    print('running f3()')
    
def main():
    print('running main()')
    print('reg --> ', reg)
    f1()
    print(f1)
    f2()
    print(f2)
    f3()
    
if __name__ == '__main__':
    main()
```

在代码中，f1和f2方法使用了reg装饰器进行装饰，f3没有，运行结果如下

```bash
running func is (<function f1 at 0x10b8f69d8>)
running f1()
running func is (<function f2 at 0x10b8f6a60>)
running f2()
running main()
reg -->  <function reg at 0x10b8f6158>
running f1()
<function f1 at 0x10b8f69d8>
running f2()
<function f2 at 0x10b8f6a60>
running f3()
```

从输出结果可以看出，装饰器在f1和f2方法调用前就运行了，运行了两次，调用reg时，传给它的参数是被装饰的函数f1和f2，而f3函数只有在main中才被调用执行

这里想强调的是，**函数装饰器在导入模块时立即执行，而被装饰的函数只在明确调用时运行**

在上面的代码中，reg装饰器原封不动的返回被装饰的函数，这种做法并非一无是处，很多python web框架使用这一的装饰器吧函数添加到某种中央注册处，例如把url模式映射到HTTP相应的函数上的注册处

一个实际的例子，超市折扣

```
promos = []

def promotion(promo_func):
    promos.append(promo_func)
    return promo_func

@promotion
def fidelity(order):
    #积分为1000以上的顾客提供5%折扣
    return order.total() * 0.5 if order.customer.fidelity >= 1000 else 0

@promotion
def bulk_item(order):
    #单个商品为20个或以上提供10%的折扣
    discount = 0
    for item in order.cart:
        if item.quantity >= 20:
            discount += item.total() * .1
    return discount

@promotion
def large_order(order):
    #订单汇总不同的商品达到10个或以上提供7%的折扣
    distinct_items = {item.product for item in order.cart}
    if len(distinct_items) >= 10:
        return order.total() * .07
    return 0

def best_promo(order):
    return max(promo(order) for promo in promos)
```

这样写有几个好处

1.促销策略函数无需使用特殊名称，如以_promo结尾
2.@promotion装饰器突出被装饰函数的作用，想临时禁用某个促销策略时，只需吧装饰器注释掉

但是，大多数装饰器会修被装饰的函数，通常的做法是定义一个内部函数，然后将其返回，替换被装饰的函数，**使用内部函数的代码几乎都要靠闭包才能正确运作**

要深入了解装饰器，就必须要理解闭包

### 4

变量作用域规则

```python
In [7]: def f1(a):
   ...:     print(a)
   ...:     print(b)
   ...:

In [8]: f1(6)
6
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-8-3ae6fc5ed790> in <module>()
----> 1 f1(6)

<ipython-input-7-8c139ea215ce> in f1(a)
      1 def f1(a):
      2     print(a)
----> 3     print(b)
      4

NameError: name 'b' is not defined
```

函数中，a为局部变量，b为全局变量，全局变量b没有赋值，报错正常

为变量b先赋值，修改一下f1方法，代码如下

```python
In [9]: b = 6
   ...: def f2(a):
   ...:     print(a)
   ...:     print(b)
   ...:     b = 9
   ...: f2(3)
   ...:
3
---------------------------------------------------------------------------
UnboundLocalError                         Traceback (most recent call last)
<ipython-input-9-fde9fff01553> in <module>()
      4     print(b)
      5     b = 9
----> 6 f2(3)

<ipython-input-9-fde9fff01553> in f2(a)
      2 def f2(a):
      3     print(a)
----> 4     print(b)
      5     b = 9
      6 f2(3)

UnboundLocalError: local variable 'b' referenced before assignment

```

报出局部变量b在赋值之前引用，原因是，变量b在f2函数定义体中赋值了，所以python编译器会认为变量b是局部变量，而不是全局变量，这不算缺陷，而是设计选择

如果在函数定义体中重新赋值了变量，想让该变量继续保持全局变量的身份，使用global关键字则可

```python
In [10]: b = 6
    ...: def f2(a):
    ...:     global b
    ...:     print(a)
    ...:     print(b)
    ...:     b = 9
    ...: f2(3)
    ...:
3
6
```

### 5

闭包指延伸了作用域的函数，其中包含函数定义体中引用，但是不在定义体中定义的非全局变量。函数是不是匿名没有关系，**关键是跟它能访问定义体之外的非全局变量**

从具体的实例如手：

```python
In [12]: def make_averager():
    ...:     series = []
    ...:
    ...:     def averager(new_value):
    ...:         series.append(new_value)
    ...:         total = sum(series)
    ...:         return total/len(series)
    ...:     return averager
    ...:

In [13]: avg = make_averager()

In [14]: avg(10)
Out[14]: 10.0

In [15]: avg(11)
Out[15]: 10.5

In [16]: avg(12)
Out[16]: 11.0
```

上面代码定义了make_averager方法，series是其中的局部变量

调用avg(10)是，make_averager函数已经返回了，而它的本地作用域也会被释放，那么series作为函数作用域中的局部变量，其中的值也会随之函数作用域的释放而被释放，但从avg(11)和avg(12)调用的结果来看，series变量中的值没有被释放，反而一直存着。












