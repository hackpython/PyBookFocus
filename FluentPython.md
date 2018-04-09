# 《流畅的python》要点笔记

>本书由奋战在Python开发一线近20年的Luciano Ramalho执笔，Victor Stinner、Alex Martelli等Python大咖担纲技术审稿人，从语言设计层面剖析编程细节，兼顾Python 3和Python 2，告诉你Python中不亲自动手实践就无法理解的语言陷阱成因和解决之道，教你写出风格地道的Python代码。

## 数据模型

### 1
len不是普通方法，而是内置方法，与__len__绑定，原因在于快速，list、str等数据结构经常会使用len方法，len快的背后是因为CPython会直接从一个C结构体中读取一个对象的长度，完全不会去调用其他方法，abs也同理

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











