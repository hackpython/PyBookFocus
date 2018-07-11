# 《量化交易之路 用python做股票量化分析》要点笔记

## 量化引言

### 什么是量化交易

普通的交易由交易者根据自身的经验或者偏好进行投资决策，量化交易通过将数据（行情历史、基本面信息及新闻资讯等）输入量化模型后，利用计算机及统计学技术方法分析数据，产生交易信号进行交易决策。

量化交易之以数学模型代替人为的主观判断，利用计算机技术从庞大的历史数据中海选出能带来超额收益的多种“大概率”事件以制定策略。

量化模型一般包括选股模块、择时模块、风险模块。

传统的投资决策不论是技术面分析还是基本面分析都属于定性分析。

定性分析通过人的思维来完成，优点是深度上占有绝对优势。

量化交易使用定量分析，它以历史数据分析为基础，利用数学、统计学等工具高效快速地进行决策。

定量分析利用计算机强大的运算能力，在广度上占有绝对优势。定量分析中使用到定性分析中很多技术，大多数情况是人的思维对定性分析的高度抽象形成模式，将模式运用到定量分析以提升广度，在**大数定理**的魔力下，与市场博弈

### 投资？投机？赌博？

赌博靠的是运气，完全无法得到概率优势。

投机需要技巧和经验，衡量得失，获取概率优势。

股票就是投资，期货就是投机。

量化交易中大多数策略是基于对历史规律的总结，在规律的基础上发现概率优势，它的最大理论依据是**人性的相似性以及人性很难改变的事实**。如果每一个瞬间的股票价格都是全体交易者对价值所达成的一种瞬间共识，那么历史的规律在今后的交易中同样具有指导意义。

传统意义上的投资目标需要一个相对较大的概率优势目标，也就是说，投资模板需要一个比较明确的预测结果。

投资是指前期投入大量资源对投资目标进行研究分析，以活动丰厚投资回报为目标，进行长期投资的行为才能称作投资，其他所有的简单分析、中短期持有行为都是投机范畴。

量化交易更倾向于投机范畴

?>量化分析主要从历史数据中判断短期价格走势概率，就如一个拳手出拳，量化模型通过此前的数据，发现拳手此时已经举拳了，那么量化模型会判断他出拳的概率会很大，这是一个短期判断，会比较准确，但是量化模型很难判断该拳手明天中午会不会吃饭，因为对于长期判断，量化模型已经丧失了数据，没有精确的数据来做概率判断，就没有让人信服的理由

### 量化交易的优势

优势：

+ 通过计算机运算能力，在市场广度分析上占有绝对优势
+ 通过历史规律的总结，在其基础上发现概率优势，形成良好投机基础

交易者常常有“交易瘾”，他们在做交易是会有快感，其交易的主要目的是寻找刺激，他们企图抓住市场中每一次价格波动，贪婪地不想放过任何一次机会。

心理学发现，交易瘾有交易决策所带来的快感造成，因此不再决策的最前沿就不会无法自拔，通过量化交易，交易者可以抽离决策前沿。

**简单即美，如在期货市场，作者最信奉的策略是3天内不能盈利便平仓**

### 避免逆势操作

股票的价值是相对的，极端来说可以认为完全没有价值，价格却是市场中买卖双方达成的共识，它是绝对的。

顺势交易是投资者必须遵循的交易模式，但大多数投资者喜欢逆势操作，比如只在股票下跌很多时买入，造成捡便宜的心理。投资者在股票下跌很多后买入股票，会有种错觉：反正很多人买的价格比我高，我不怕，要死大家一起死。相反，投资者在股票价格突破后不敢买入：我不能在最高点买入，买了后，股票价格下跌就只有我一个人死了。

均值回复模式：假设之前的股价上涨只是暂时的，价格会恢复到一个相对正常的水平，也就说随后的一段时间内股价会下跌，其理论依据为价格将围绕价值上下波动。

### 避免重仓交易

重仓交易的危害不止是让交易者产生重大亏损这么简单。

交易中经常会出现买入后初始阶段并没有盈利的情况，在仓位小的情况下加上合理的止损策略，完全可以等待走势步入正轨，但过大的仓位会导致资金的大波动，交易者因无法承受资金波动选择过早离场，因而间接步入短线的陷阱。

很多的重仓交易是交易者选择向下加仓的结果，集交易者不断加仓已经亏损很多的头寸，企图通过降低成本来实现交易反转，间接步入逆势的陷阱

?>头寸：款项的意思，指投资者拥有或借用的资金数量

交易中应追求**让利润尽情奔跑，让亏损尽快止损**，而不是追求胜率，胜率毫无意义

量化交易策略都会完成回测及回测结果度量，即通过回测及回测结果验证我们的交易策略的可行性，是具有概率上的优势的交易策略，它具有正期望的投资回报。

### 从历史中验证交易策略是否可行

开发交易策略都要验证策略是否可行，通过量化交易将需要验证的策略实现后进行历史回测，通过对回测结果进行度量即可大致清楚该测了是否有效以及有效范围

?>回测面临一个重要问题就是，我们开发的交易策略，在回测过程中，进行交易，这些交易如果是在真实世界中是会产生复杂反应的，而回测系统毕竟只是模拟过去的交易环境无法完全捕捉到所有的变化，所有回测和真实情况还是有差距的

?>交易市场是个二级混沌的，所谓二级混沌就是对交易市场本身的研究或交互都会影响到市场整体，比如你发表一篇读某个股票的看法，某人可能看了你这篇分析就买入了这只股票，此时市场就发生变化了，想天气就是一级混沌的，对天气的预测观察不会让天气发现太大的变化，所以我们对天气的预测会越来越准确，但预测市场就会更加困难

交易者实际上并不需要过多关注盘间的价格波动，这样才能客观地对交易进行控制，不会被过多无意义的干扰打乱节奏。

### 量化交易正确认识

实际上，真正的基本面分析的进入门槛很高的，需要大量的资源和调研资金，并不适合个人投资者。

基本面分析的结果为长期预测结构，集通过大量的资料进行基本面调研后，寻找在长期投资期间具有丰厚回报的目标。

基本面分析以统计学为基础，通过寻找概率优势进行分析，分析结果为概率。

技术分析在中短期投资区间内有效，很多量化交易策略是基于浅层基本面数据分析+技术分析实现的。

**一种分析方法知道的人多了，使用的人多了，自然也就失效了**

在投资领域中没有一种方式就可以永远盈利，唯一不变的是变化和手续费。量化分析在这方面反而有自己的优势，分析变化，调整策略，通过数据反向指导人的思维调整定性分析方法，再此抽象到定量分析。

### 量化交易没有神奇魔法

现在所有的人工智能都属于弱人工智能（现在：2018-5-13）

阿尔法围棋的本质是多个弱人工智能效力叠加，通过游戏规则取胜。

虽然市场无法预测，但市场并不是杂乱无章的，由于市场参与者的非理性行为（有效市场假说不成立），通过历史数据发现规律，一定可以获得一些概率上的优势，作者认为在预测和混沌间存在一个种状态，这种状态可以使用概率来描述。即通过算法来找到这些概率的分布，预测市场的混沌。

### 不要抱有不劳而获的幻想

量化交易的基础依旧是交易，交易付出的代价是相当高的：成功的交易者不但需要付出资金成本，忍受交易的各种精神折磨，反人性的训练自己的交易技术，而且主要是要独立思考，不断否定自自我，不断付出大量时间去学习更新自己，是刀尖上舔学的工作

有效市场假说认为市场价格波动是随机的，交易者不可能持续从市场中获利。但市场并不是完全有效的。

影响市场的原因无限多，且非线性关系，即无限解系统，所以无法通过复杂化系统来实现预测交易的效果，不论使用技术有多复杂高深，战胜市场的唯一方式依然是获得概率优势。

只有简单的策略才能在长期投资中保存高度的稳定概率优势。

### 认清市场，认清自己

自动化交易占据美国股票市场 60% 以上的成交量。在美国，很多高频交易为了通信速度能有几毫秒的提升，不惜在太平洋底部搭建自己的通信网络。

选定交易品种，股票、期权、期货、外汇、比特币等都要考量，针对你的资金量、硬件设备等作出取舍。比如期货交易由于市场本身的特性，很多交易策略都是针对盘口数据做出快速反应，这样，硬件设备确实是能否获利的关键，高平交易也不适合个人投资者。但很多交易策略对硬件设备和速度要求没有那么高。

大机构的大资金在市场中，每一次出手时需要考虑冲击成本，冲击成本是指在交易中需要迅速而且大规模地买进或卖出证券，未能按照预定价位成交，从而多支付的成本.

?>资金量太大，没有那么多可以买，所以交易品种价格上涨了

冲击成本被认为机构大户难以摆脱的致命伤等不利因素。

要认清自己的策略与资金量是否匹配，尽量不要使用杠杆，杠杆放大了风险与收益的同时，也放大了希望和恐惧，期货市场投资者平均存活周期为6个月。

个人投资者需要对市场有足够的认知、敬畏，认清自己的资金量是不是可以满足市场的最低入门门槛，并且杠杆越高的市场要求投资者具有越高的自律性，控制自己的贪婪，不和市场较劲。

### 量化交易的目的

自由

?> 只有抛弃一切，才能获得自由 《搏击俱乐部》

我们拥有的其实只有时间

## 量化语言 --- python

python语言里一些高级语法，如混入类mixmins、元编程metaclass和装饰器，他们都是一些最优解决方案中必不可少的技术，甚至描述器这个一般用不到的技术，在编写大型项目中，也使项目结构更加优雅。

### 1
isinstance() --> 验证某个对视是否是某个类型

### 2
python中字符串是不可变对象（immutable），一般对字符串操作的API都是返回一个新的字符串。

通过id()函数可以获得对象的内存地址，如果两个对象的内存地址是一样的，那么这两个对象就是一个对象。

### 3
列表list是一种**有序**容器，可以对元素进行增、删、改操作

集合set是一个**无序**容器，且集合中的元素没有重复

### 4
zip的效果是同时迭代多个序列，每次分别从一个序列中取一个元素，一旦其中某个序列到达结尾，则迭代结束

```python
In [51]: m = [i for i in range(10)]

In [52]: m1 = [x for x in range(5)]

In [53]: m
Out[53]: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

In [54]: m1
Out[54]: [0, 1, 2, 3, 4]

In [55]: m2 = [(a,b) for a,b in zip(m,m1)]

In [56]: m2
Out[56]: [(0, 0), (1, 1), (2, 2), (3, 3), (4, 4)]
```

### 5
可命名元祖：nametuple

```python
n [59]: from collections import namedtuple

In [60]: s = namedtuple('s',('a','b'))

In [61]: sl = [s(a,b) for a,b in zip(m,m1)]

In [62]: sl
Out[62]: [s(a=0, b=0), s(a=1, b=1), s(a=2, b=2), s(a=3, b=3), s(a=4, b=4)]

In [63]: sl[0].a
Out[63]: 0
```

### 6
字典推到式

```python
In [67]: sb = {a:b for a,b in zip(m,m1)}

In [68]: sb
Out[68]: {0: 0, 1: 1, 2: 2, 3: 3, 4: 4}
```

dict使用key-value存储，可以通过key快速查询value，使用keys()和values()函数可以分别返回字典中的key列表与value列表

### 7

有序字典：OrderedDict

```python
In [71]: from collections import OrderedDict

In [72]: sb = OrderedDict((a,b) for a,b in zip(m,m1))

In [73]: sb
Out[73]: OrderedDict([(0, 0), (1, 1), (2, 2), (3, 3), (4, 4)])

In [74]: sb.keys()
Out[74]: odict_keys([0, 1, 2, 3, 4])
```

### 8

lambda函数轻量级完成了函数的任务，特别是针对reduce()和map()等高级函数

找字典中，第二个大的元素

```python
In [73]: sb
Out[73]: OrderedDict([(0, 0), (1, 1), (2, 2), (3, 3), (4, 4)])

In [75]: find_second_max_lambda = lambda sb:sorted(zip(sb.values(), sb.keys()))[-2]

In [76]: find_second_max_lambda(sb)
Out[76]: (3, 3)
```

python中函数可以返回多个返回值，但实际上仍然是一个返回值，只不过返回值通过打包成一个tuple元组返回，实现多个返回值

### 9

常见高级函数：

+ map()函数：接受两个参数，一个是函数，一个是序列，map()把传入的函数依次作用于序列的每个元素，并把结果作为新的序列返回
+ filter()函数：接受两个参数，一个是函数，一个是序列，filter()把传入的函数依次作用于每个序列中的每个元素上，根据返回值是True还说False决定保留还是丢弃元素，结果序列是所有返回值为True的子集
+ reduce()函数：把一个函数作用与一个序列上，该函数必须接受两个参数，其中reduce()函数会**把结果继续和序列的下一个元素做累加计算**，reduce()函数只返回值结果，而非序列

```python
# 过滤出2的倍数
In [7]: f = filter(lambda a:a%2==0, [i for i in range(10)])

In [8]: [i for i in f]
Out[8]: [0, 2, 4, 6, 8]
# list中int转str
In [10]: m = map(str, [i for i in range(10)])

In [11]: [i for i in m]
Out[11]: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']

# 累加1到时
In [19]: reduce(lambda x,y:x+y,range(10))
Out[19]: 45
```

### 10

偏函数，某个函数参数过多，使用时很容易产生错误，可以使用**functions.partial()**方法来固化函数中的某几个参数，从而创建一个新的函数，这样在调用时更加简单

```python
In [1]: def mod(a,b):
   ...:     return a%b
   ...:

In [2]: mod(8,4)
Out[2]: 0

In [3]: mod(8,3)
Out[3]: 2

In [4]: from functools import partial

# 固定mod函数的b参数为3
In [5]: mod_3 = partial(mod, b=3)

In [6]: mod_3(8)
Out[6]: 2
```

### 11
python中类中最大的特点是，每个类方法的第一个参数我都是以self开始，后面是其他参数。

在python中没有语法上强制的访问控制，没有类似于java的private私有、protect受保护、public公有的方法显示声明，而是通过遵循一定的属性和方法名规则来达到这个效果。

编写一个类，用上上面提过的方法

```python
from collections import namedtuple, OrderedDict
from functools import reduce

class StockTradeDays(object):
    def __init__(self, price_array, start_date, date_array=None):
        self.__price_array = price_array
        self.__date_array = self._init_days(start_date, date_array)
        self.__change_arrray = self._init_change()
        self.stock_dict = self._init_stock_dict()

    def _init_change(self):
        '''
        从price_array生成change_array
        '''
        price_float_array = [float(price_str) for price_str in self.__price_array]
        # 时间平移成两个错开的收盘价格序列，通过zip打包成一个新序列，其中每个元素为相邻的两个收盘价格
        pp_array = [(price1, price2) for price1, price2 in zip(price_float_array[:-1], price_float_array[1:])]
        change_array = map(lambda pp:reduce(lambda a,b: round((b-a)/a, 3), pp), pp_array)
        change_array = [i for i in change_array]
        # 插入数据，将第一天涨跌幅设置为0
        change_array.insert(0,0)
        return change_array

    def _init_days(self, start_date, date_array):
        '''
        初始化日期序列
        '''
        if date_array is None:
            date_array = [str(start_date + ind) for ind,_ in enumerate(self.__price_array)]
        else:
            date_array = [str(date) for date in date_array]
        return date_array

    def _init_stock_dict(self):
        '''
        使用namedtuple和OrderedDict将结果合并
        :return:
        '''
        stock_namedtuple = namedtuple('stock',('date', 'price', 'change'))
        # 先构造成namedtuple，再构造成OrderedDict
        stock_dict = OrderedDict(
            (date, stock_namedtuple(date, price, change))
            for date, price, change in zip(self.__date_array, self.__price_array, self.__change_arrray)
        )
        return stock_dict

    def filter_stock(self, want_up=True, want_calc_sum=False):
        '''
        筛选结果子集
        '''
        filter_func = (lambda day:day.change >0) if want_up else (lambda day:day.change<0)
        # 返回三目运算的结果
        want_days = filter(filter_func, self.stock_dict.values())
        if not want_calc_sum:
            return want_days
        change_sum = 0.0
        for day in want_days:
            change_sum += day.change
        return change_sum

    def __str__(self):
        # 打印时类显示
        return str(self.stock_dict)

    def __iter__(self):
        # 迭代
        for key in self.stock_dict:
            yield  self.stock_dict[key]

    def __getitem__(self, item):
        # 通过下标获取
        date_key = self.__date_array[item]
        return self.stock_dict[date_key]

    def __len__(self):
        # 长度
        return len(self.stock_dict)

if __name__ == '__main__':
    price_array = '30.14,29.58,26.36,32.56,32.82'.split(',')
    date_base = 20170118
    trade_days = StockTradeDays(price_array, date_base)
    print(trade_days)
```

结果：

```bash
OrderedDict([('20170118', stock(date='20170118', price='30.14', change=0)), ('20170119', stock(date='20170119', price='29.58', change=-0.019)), ('20170120', stock(date='20170120', price='26.36', change=-0.109)), ('20170121', stock(date='20170121', price='32.56', change=0.235)), ('20170122', stock(date='20170122', price='32.82', change=0.008))])
```

回头看上面的StockTradeDays类实现了长度获取，迭代等功能

python中获取字符串长度或者数组的长度都使用len(str)和len(list)，可以用len()方法计算长度的对象，其内部都实现了__len__(),len()内建函数就是调用这个对象的__len__()来获得对象的长度

python中list、tuple、dict都可以通过for in进行遍历迭代序列中的每个元素，这种特性称为可迭代

+ 要判断对象是否支持迭代操作，可使用 collections.Iterable;
+ 自定义的类只要实现了__iter__()方法就支持迭代操作

StockTradeDays类中就实现了__iter__()方法，其中通过yield进行返回

类似，只要实现了__getitem__()，该类就支持索引获取了

使用真实数据试一下，这里使用tushare提供的数据，而书中使用的abupy.MarketBu.ABuSymbolPd.py提供的数据，虽然数据源不同，但没有实际差别

?>  Tushare是一个免费、开源的python财经数据接口包。主要实现对股票等金融数据从数据采集、清洗加工 到 数据存储的过程，能够为金融分析人员提供快速、整洁、和多样的便于分析的数据，为他们在数据获取方面极大地减轻工作量，使他们更加专注于策略和模型的研究与实现上。考虑到Python pandas包在金融量化分析中体现出的优势，Tushare返回的绝大部分的数据格式都是pandas DataFrame类型，非常便于用pandas/NumPy/Matplotlib进行数据分析和可视化。

客观说，tushare是很好用的财经数据接口了，而且在逐步提供数据的准确度，公司使用的高精准数据都是需要购买的，感谢开源，感谢tushare

tushare不支持港股、美股等国外股票（因为国外的金融数据都很好获取，不想国内有点乱象丛生的感觉），这里通过tushare获取 建设银行 这支股票的2016-01-01到2018-01-01这两年的数据

```python
if __name__ == '__main__':
    # 获得建设银行的数据
    data = ts.get_k_data('601939',start='2016-01-01', end='2018-01-01')
    price_array = data['close'] #收盘价
    date_array = data['date'] #日期
    date_base = 20160101
    trade_days = StockTradeDays(price_array,date_base,date_array)
    print('长度{}'.format(len(trade_days)))
    print('最后一天叫数据{}'.format(trade_days[-1]))
```

上面代码使用了get_k_data()接口，[文档](https://mp.weixin.qq.com/s?__biz=MzAwOTgzMDk5Ng==&mid=2650833972&idx=1&sn=4de9f9ee81bc8bf85d1e0a4a8f79b0de&chksm=80adb30fb7da3a19817c72ff6f715ee91d6e342eb0402e860e171993bb0293bc4097e2dc4fe9&mpshare=1&scene=1&srcid=1106BPAdPiPCnj6m2Xyt5p2M#wechat_redirect)

get_k_date()接口返回的事pandas的DataFrame类型，直接从其中取出我们需要的列，每一列其实是pandas中的Series类型，Series类型支持迭代，所以可以直接作为参数传递给StockTradeDays类

结果:

```bash
长度488
最后一天叫数据stock(date='2017-12-29', price=7.6799999999999997, change=0.017)
```

### 12

**逆势、短线、重仓是交易失败的三大原因**

### 13
python继承与多态

通过ABC模块实现接口或抽象类，并通过执行类型检查来确保子类实现了某些特定的方法

```python
from abc import ABCMeta, abstractmethod
import six

class TradeStrategyBase(six.with_metaclass(ABCMeta, object)):
    '''
    交易策略抽象基类
    '''
    @abstractmethod
    def buy_strategy(self, *args, **kwargs):
        # 买入策略
        pass
    
    @abstractmethod
    def sell_strategy(self, *args, **kwargs):
        # 卖出策略基类
        pass
```

TradeStrategyBase类通过继承ABCMeta， @abstractmethod声明方法为接口，装饰器的一种写法

装饰器是在python闭包技术基础上再次底层封装的技术，建立在Python里一切皆为对象的根本上

**多个装饰器修饰一个方法时，需要注意装饰器的顺序**

为了能让一个函数接受任意数量的位置参数，可以使用\*参数，它的数据结构类型为list列表

为了接受任意数量的关键字参数，可以使用 \**开头的参数，它的数据结构为dict字典

?>在实际开发中，除了最底层的基类，能不用*args和\**kwargs就不要用，因为当你的写好了项目交个其他人维护时，因为这两个参数可以传任意值进入，又不是维护项目的人自己写的，所以从代码层面，它更本不知道你往里传入了什么参数，无形中大大增加了维护成本

接着编写具体的量化策略 TradeStrategy1，继承 TradeStrategyBase实现buy_strategy()与sell_strategy()接口，具体的交易策略为：当股价上涨一个阈值时买入股票并持有相应天数，具体代码如下：

```python
class TradeStrategy1(TradeStrategyBase):
    '''
    交易策略1：追涨策略，当股价上涨一个阈值默认为7%是买入股票并持有 s_keep_stock_threshold(20)天
    '''
    s_keep_stock_threshold = 20
    def __init__(self):
        self.keep_stock_day = 0
        # 固定上涨幅度作为买入策略的阈值
        self.__buy_change_threshold = 0.05

    def buy_strategy(self, trade_ind, trade_day, trade_days):
        if self.keep_stock_day == 0 and trade_day.change > self.__buy_change_threshold:
            # 没有持有股票的时候self.keep_stock_day == 0，且符合买入条件上涨一个阈值，买入
            self.keep_stock_day += 1
        elif self.keep_stock_day > 0: #持有股票，持有股票天数递增
            self.keep_stock_day += 1

    def sell_strategy(self, trade_ind, trade_day, trade_days):
        if self.keep_stock_day >= TradeStrategy1.s_keep_stock_threshold:
            # 当持有股票天数超过阈值s_keep_stock_threshold则卖出股票
            self.keep_stock_day = 0

    @property
    def buy_change_threshold(self):
        return self.__buy_change_threshold

    @buy_change_threshold.setter
    def buy_change_threshold(self, buy_change_threshold):
        if not isinstance (buy_change_threshold, float):
            #上涨阈值需为float类型
            raise TypeError('buy_change_threshold must be float')
        # 上涨阈值只取小数点后两位
        self.__buy_change_threshold = round(buy_change_threshold, 2)
```

上面就是继承了TradeStrategyBase这个抽象类的策略，其实现了buy_strategy()买方法和sell_strategy()卖方法，买卖的逻辑也很简单

买逻辑：

+ 如果现在没有持有股票（股票持有天数为0）且当天股票价格的上涨幅度大于设定的阈值，则买入（持有股票就是股票持有天数加1
+ 如果本来就持有股票，那就继续持有（股票持有天数加1）

卖逻辑：

+ 如果持有股票天数大于我们设置的最大天数，也就是20天，那就卖出股票，股票持有天数归零

策略写好了，当然要知道这个策略有没有效果，可以将写好的策略放到历史数据中，比如建设银行前两年的历史数据，看看这个简单的追涨策略有无效果，简单来说，就是做 `回测`

回测是量化中重要的部分，如果策略连回测都过不了，说明就没有价值了，这里不用建设银行这支股票，因为它太平稳了，不涨不跌，我们写的追涨策略并不适用于它，这里选择汇川技术这支股票2016-01-01到2018-01-01作为回测数据

首先先编写一个简单的交易回测系统，如下

```python
class TradeLoopBack(object):
    '''
    交易回测系统
    使用前面封装的StockTradeDays类和本节编写的交易策略类
    TradeStrategyBase类初始化交易系统
    :param trade_days:交易数据序列 StockTradeDays
    :param trade_strategy:交易策略 TradeStrategyBase
    '''
    def __init__(self, trade_days, trade_strategy):
        self.trade_days = trade_days
        self.trade_strategy = trade_strategy
        self.profit_array = [] #盈亏结果序列

    def execute_trade(self):
        '''
        执行回测
        '''
        for ind, day in enumerate(self.trade_days):
            '''
            以时间驱动，完成交易回测
            '''
            if self.trade_strategy.keep_stock_day > 0:
                # 如果持有股票，加入交易盈亏结果序列
                self.profit_array.append(day.change)

            if hasattr(self.trade_strategy, 'buy_strategy'):
                # 买入策略执行
                self.trade_strategy.buy_strategy(ind, day, self.trade_days)

            if hasattr(self.trade_strategy, 'sell_strategy'):
               # 卖出策略执行
                self.trade_strategy.sell_strategy(ind, day, self.trade_days)
```

代码不复杂，但可能不明白，为啥这样写就是一个回测系统了？所谓书里的每个字都认识，连成一句后，就不知道想表达什么

但仔细看回测系统的代码还是可以理解

首先就是循环历史数据trade_days，trade_days是StockTradeDays类的实例，每一天都会返回namedtuple类型的数据，如： `stock(date='2017-12-29', price=7.6799999999999997, change=0.017)`，然后回测系统先判断有没有持有股票，持有了就记录下这天股票价格的变动，也就是change中的数据，然后调用策略中的买入和卖出操作

当回测系统迭代完所有历史数据后，就记录下来，回测的这个策略持有股票时，当天的股票的变动情况，change>0说明当天赚了change元，change<0说明当天亏了change元

需要注意的是，该回测系统仅仅从盈亏角度来衡量策略，是最简单的回测系统，真正的回测系统要考虑的参数更多，实现更加复杂

策略写好了，回测系统写好了，接着就通过tushare获取历史数据构建StockTradeDays类来运行一波代码

```
import tushare as ts
from quant2 import StockTradeDays
from functools import reduce

#---画图------
import seaborn as sns
import numpy as np
import matplotlib.pyplot as plt

if __name__ == '__main__':
    data = ts.get_k_data('300124', start='2016-01-01', end='2018-01-01')
    trade_days = StockTradeDays(data['close'], 20160101, data['date'])
    trade_loop_back = TradeLoopBack(trade_days, TradeStrategy1())
    trade_loop_back.execute_trade()
    if trade_loop_back.profit_array:
        print('回测策略 1 总盈亏为：{}%'.format(
            reduce(lambda a, b: a+b, trade_loop_back.profit_array)*100
        ))

    sns.set_context(rc= {'figure.figsize':(14,7)})
    plt.plot(np.array(trade_loop_back.profit_array).cumsum())
    plt.show()
```

首先实例化StockTradeDays类和TradeStrategy1类，然后通过这两个实例化TradeLoopBack，执行回测后，计算这个策略的总盈利，也就是追涨策略从2016-01-01日到2018-01-01日期间，反复买卖汇川技术的股票，最后有没有赚到钱

最后还通过可视化显示出追涨策略这两年间盈亏率的变化情况

运行结果：

```
回测策略 1 总盈亏为：7.299999999999994%
```

追涨策略盈亏率可视化

![](http://p6un02lk4.bkt.clouddn.com/quant_1.png)

从结果上看，使用追涨策略在2016-01-01到2018-01-01期间购买汇川技术这支股票，两年下来，你盈利的7.2个点，其实盈利效果不怎么样，简单想想2年时间人民币通货膨胀。总盈利就是过去两年间每天的盈利之和

从可视化的图中可以看出，追涨策略对于汇川技术这支股票一开始是亏损的，到2017年才开始高歌猛进。一个策略并不是适合市场上所有股票的，我选择汇川技术是因为汇川技术2017年表现不错，如果使用追涨策略购买建设银行的股票，就无法盈利了。

### 14
静态方法

在TradeStrategy1追涨策略这个类中使用了@property

```python
@property
    def buy_change_threshold(self):
        return self.__buy_change_threshold

    @buy_change_threshold.setter
    def buy_change_threshold(self, buy_change_threshold):
        ...
```

第一个@property方法是一个getter函数，它使buy_change_threshold成为一个属性，类实例可以直接像调用属性那样通过 `.` 直接调用这个方法，一般用于获得某个值

一般有获得就有设置，@buy_change_threshold.setter将其装饰的方法同样变为一个属性，类实例可以向为属性变量赋值的方式直接给方法赋值

```python
t = TradeStrategy1()
#访问
print(t.buy_change_threshold)
#设置
t.buy_change_threshold = 10
```

使用@property的目的是给实例增加除访问与修改之外的其他处理逻辑，如buy_change_threshold做了类型检查，将float阈值保留两位小数的操作，**没有必要写没人做任何其他额外操作的property，因为使用@property会降低效率**

将交易买入阈值从0.05改成0.01，即超过1%的当日涨跌幅才作为买入信号

```python
if __name__ == '__main__':
    data = ts.get_k_data('300124', start='2016-01-01', end='2018-01-01')
    trade_days = StockTradeDays(data['close'], 20160101, data['date'])
    trade_streategy1 = TradeStrategy1()
    # 将买入阈值改为0.01
    trade_streategy1.buy_change_threshold = 0.01
    trade_loop_back = TradeLoopBack(trade_days, trade_streategy1)
    trade_loop_back.execute_trade()
    if trade_loop_back.profit_array:
        print('回测策略 1 总盈亏为：{}%'.format(
            reduce(lambda a, b: a+b, trade_loop_back.profit_array)*100
        ))

    sns.set_context(rc= {'figure.figsize':(14,7)})
    plt.plot(np.array(trade_loop_back.profit_array).cumsum())
    plt.show()
```

结果：

```bash
回测策略 1 总盈亏为：22.599999999999998%
```

![](http://p6un02lk4.bkt.clouddn.com/quant_2.png)

总盈亏从7.2%涨到了22.59%

### 15
接着实现一个均值回复策略，主体逻辑也很简单，当股价连续两个交易日下跌，且下跌幅度超过一个阈值（默认为-10%）时，买入股票并持有一定的天数，代码如下

```python
class TradeStrategy2(TradeStrategyBase):
    '''
    交易策略2：均值回复策略，当股价连续两个交易日下跌，且下跌幅度超过阈值，则买入股票持有s_keep_stock_threshold天
    '''

    #买入后持有天数
    s_keep_stock_threshold = 10

    # 下跌买入阈值
    s_buy_change_threshold = -0.10

    def __init__(self):
        self.keep_stock_day = 0

    def buy_strategy(self, trade_ind, trade_day, trade_days):
        if self.keep_stock_day == 0 and trade_ind >= 1:
            today_down = trade_day.change < 0 #今日下跌，today_down为True
            yesterday_down = trade_days[trade_ind - 1].change < 0 #昨日下跌
            down_rate = trade_day.change + trade_days[trade_ind - 1].change #两天下跌总量
            # 如果今日和昨日连续两天都下跌，且下跌幅度大于阈值，则买入
            if today_down and yesterday_down and down_rate < TradeStrategy2.s_buy_change_threshold:
                self.keep_stock_day += 1
        elif self.keep_stock_day > 0:
            self.keep_stock_day += 1

    def sell_strategy(self, trade_ind, trade_day, trade_days):
        # 股票持有天数大于阈值，卖出
        if self.keep_stock_day >= TradeStrategy2.s_keep_stock_threshold:
            self.keep_stock_day = 0

    @classmethod
    def set_keep_stock_threshold(cls, keep_stock_threshold):
        cls.s_keep_stock_threshold = keep_stock_threshold

    @staticmethod
    def set_buy_change_threshold(buy_changee_threshold):
        TradeStrategy2.s_buy_change_threshold = buy_changee_threshold
```

调用一下，依旧选择汇川技术这支股票

```python
if __name__ == '__main__':
    data = ts.get_k_data('300124', start='2016-01-01', end='2018-01-01')
    trade_days = StockTradeDays(data['close'], 20160101, data['date'])
    trade_strategy2 = TradeStrategy2()
    trade_loop_back = TradeLoopBack(trade_days, trade_strategy2)
    trade_loop_back.execute_trade()
    print('回测策略2 总盈亏为 {}%'.format(reduce(lambda a, b: a+b, trade_loop_back.profit_array) * 100))
    sns.set_context(rc= {'figure.figsize':(14,7)})
    plt.plot(np.array(trade_loop_back.profit_array).cumsum())
    plt.show()
```

运行结果

```bash
回测策略2 总盈亏为 0.10000000000000062%
```

![](http://p6un02lk4.bkt.clouddn.com/quant_3.png)

总盈亏为0.1%，非常差劲，将钱投进了风险较高的股市，收益比存入银行还底

从可视化盈亏曲线可以看出有一段持续下跌的过程，在量化交易中成为最大回撤。**最大回撤是一个度量策略优劣的重要因素**

### 16
在均值回复策略的代码中，使用了静态方法@staticmethod和类方法@classmethod

+ @staticmethod：不需要任何参数
+ @classmethod：不需要self参数，但第一个参数需要表示自身类的cls参数
+ @staticmethod装饰的方法中药使用这个类的变量，只能直接使用`类名.属性名`或`类名.方法名`
+ @classmethod装饰的方法可以使用cls来访问类中的变量，其优点是**避免硬编码**

通过策略2中的类方法和今天方法修改一下买入阈值和持股天数，将买入阈值改为-0.05，即连续两天下跌5%则买入，持股天数改为20天

```python
if __name__ == '__main__':
    data = ts.get_k_data('300124', start='2016-01-01', end='2018-01-01')
    trade_days = StockTradeDays(data['close'], 20160101, data['date'])
    trade_strategy2 = TradeStrategy2()
    # 直接通过类名调用，股票持有天数阈值设为20天
    TradeStrategy2.set_keep_stock_threshold(20)
    # 下跌买入阈值改为0.05
    TradeStrategy2.set_buy_change_threshold(-0.05)
    trade_loop_back = TradeLoopBack(trade_days, trade_strategy2)
    trade_loop_back.execute_trade()
    print('回测策略2 总盈亏为 {}%'.format(reduce(lambda a, b: a+b, trade_loop_back.profit_array) * 100))
    sns.set_context(rc= {'figure.figsize':(14,7)})
    plt.plot(np.array(trade_loop_back.profit_array).cumsum())
    plt.show()
```

运行结果如下：

```bash
回测策略2 总盈亏为 25.29999999999999%
```

![](http://p6un02lk4.bkt.clouddn.com/quant_4.png)

总盈利为25.29%，修改了参数后，效果不错

### 17
标准库中 `itertools` 提供了很多生成循环器的工具，其中很重要的用途是生成集合中所有可能方式的元素排列和组合。**在量化数据处理中经常会使用itertools来完成数据的各种排列组合，从而寻找最优参数。**

1.permutations()函数，获得序列中元素的全排列，也就是考虑顺序的，不同的顺序认为是不同类型

```python
In [1]: import itertools

In [2]: items = [1,2,3,4]

In [3]: for item in itertools.permutations(items):
   ...:     print(item)
   ...:
(1, 2, 3, 4)
(1, 2, 4, 3)
(1, 3, 2, 4)
(1, 3, 4, 2)
(1, 4, 2, 3)
(1, 4, 3, 2)
(2, 1, 3, 4)
(2, 1, 4, 3)
(2, 3, 1, 4)
(2, 3, 4, 1)
(2, 4, 1, 3)
(2, 4, 3, 1)
(3, 1, 2, 4)
(3, 1, 4, 2)
(3, 2, 1, 4)
(3, 2, 4, 1)
(3, 4, 1, 2)
(3, 4, 2, 1)
(4, 1, 2, 3)
(4, 1, 3, 2)
(4, 2, 1, 3)
(4, 2, 3, 1)
(4, 3, 1, 2)
(4, 3, 2, 1)
```

2.combinations()函数，不考虑顺序，不放回数据

```python
In [4]: for item in itertools.combinations(items, 3):
   ...:     print(item)
   ...:
(1, 2, 3)
(1, 2, 4)
(1, 3, 4)
(2, 3, 4)
```

3.combinations_with_replacement()函数，不考虑顺序，有放回数据

```python
In [6]: for item in itertools.combinations_with_replacement(items, 3):
   ...:     print(item)
   ...:
(1, 1, 1)
(1, 1, 2)
(1, 1, 3)
(1, 1, 4)
(1, 2, 2)
(1, 2, 3)
(1, 2, 4)
(1, 3, 3)
(1, 3, 4)
(1, 4, 4)
(2, 2, 2)
(2, 2, 3)
(2, 2, 4)
(2, 3, 3)
(2, 3, 4)
(2, 4, 4)
(3, 3, 3)
(3, 3, 4)
(3, 4, 4)
(4, 4, 4)
```

4.product()函数，笛卡尔积

该方法会针对多个输入序列进行排列组合

```python
In [7]: abc = ['a','b','c']

In [8]: efg = ['e','f','g']

In [9]: for item in itertools.product(abc, efg):
   ...:     print(item)
   ...:
('a', 'e')
('a', 'f')
('a', 'g')
('b', 'e')
('b', 'f')
('b', 'g')
('c', 'e')
('c', 'f')
('c', 'g')
```

**在量化中，通过参数组合来寻找最优参数是一般会使用笛卡尔积**

### 18

使用笛卡尔积求出TradeStrategy2均值回复策略的最优参数，即求出下跌幅度买入阈值和买入股票后持有天数如何取值，可以让该策略最终盈利最大化

首先将将修改TradeStrategy2策略下跌买入阈值和持有天数的代码以及执行回测的代码整合成一个新的函数 `calc()`，该函数输入参数有两个，分别是`持股天数`和`下跌买入阈值`，输出返回值为3个，分别是盈亏情况、输入的持股天数和买入阈值

具体代码如下：

```python
import tushare as ts
from quant2 import StockTradeDays
from functools import reduce

data = ts.get_k_data('300124', start='2016-01-01', end='2018-01-01')
trade_days = StockTradeDays(data['close'], 20160101, data['date'])

def calc(keep_stock_threshold, buy_chagne_threshold):
    '''
    :param keep_stock_threshold: 持股天数
    :param buy_chagne_threshold: 下跌买入阈值
    :return: 盈亏情况、输入的持股天数、输入的下跌买入阈值
    '''
    #实例化策略2，均值回复策略
    trade_strategy2 = TradeStrategy2()
    #通过类方法设置买入后持股天数
    TradeStrategy2.set_keep_stock_threshold(keep_stock_threshold)
    #通过静态方法设置下跌买入阈值
    TradeStrategy2.set_buy_change_threshold(buy_chagne_threshold)
    #进行回测
    trade_loop_back = TradeLoopBack(trade_days, trade_strategy2)
    trade_loop_back.execute_trade()
    profit = 0.0 if len(trade_loop_back.profit_array) == 0 else reduce(lambda a, b: a+b, trade_loop_back.profit_array)
    return profit, keep_stock_threshold, buy_chagne_threshold

#测试一下是否可以正常运行
print(calc(20, -0.05))
```

正常运行，结果为：

```bash
(0.2529999999999999, 20, -0.05)
```

笛卡尔积求最优属于有限参数范围内求最优的问题，即将有限个参数形成的集合，多个有限集合进行笛卡尔积，寻找问题最优的参数。

通过range()函数使具体的参数形成有限集合，然后再通过product()函数计算笛卡尔积，再通过calc获得结果

```python
import itertools

keep_stock_list = range(2,30,2) #持股天数
buy_change_list = [buy_change /100.0 for buy_change in range(-5,-16,-1)] #下跌买入阈值

res = []
for keep_stock_threshold, buy_change_threshold in itertools.product(keep_stock_list, buy_change_list):
    res.append(calc(keep_stock_threshold, buy_change_threshold))

# [::-1]将整个排序的结果反转，反转后盈亏收益从最高向最低开始排序，[:10]取出收益最高的前10个组合查看
print(sorted(res)[::-1][:10])
```

运行结果：

```bash
[(0.45400000000000007, 28, -0.06), 
(0.381, 28, -0.07), 
(0.36100000000000004, 26, -0.05),
(0.33300000000000013, 24, -0.06), (0.3320000000000001, 18, -0.05),
(0.33199999999999996, 22, -0.06),
(0.32200000000000006, 16, -0.05), (0.3159999999999999, 26, -0.06),
(0.301, 24, -0.07), 
(0.298, 24, -0.05)]
```

可以看出持股天数28天，下跌阈值为-0.06的组合方式盈亏收益最高，收益达45.4%。这个最优结果只是在设定的keep_stock_list和buy_change_list给定的参数中排列组合最优的参数，**不代表绝对最优**

可视化一下这个最优组合看看

![](http://p6un02lk4.bkt.clouddn.com/quant_5.png)

### 19
笛卡尔积对两个参数排列组合有154种组合方式，由于这里只是简单的回测，没有运行复杂的计算，没有繁多的I/O操作，使用for循环串行计算每组参数的结果也没有速度上的问题。

但是真实的回测有复杂的计算，而且有很多I/O操作，如果不适用多线程、多进程的形式，就会有效率问题

多线程时候处理I/O密集型任何和并发执行的租售操作

多进程处理处理并行的计算密集型任务

使用多进程完成寻找最优参数的相同任务

```python
from concurrent.futures.process import ProcessPoolExecutor

res = []
# 回调函数，通过add_donw_callback任务完成后调用
def when_done(r):
    # when_done在主进程中运行
    res.append(r.result())

with ProcessPoolExecutor() as pool:
    for keep_stock_threshold, buy_change_threshold in itertools.product(keep_stock_list,buy_change_list):
        '''
        submit提交任务：将calc函数和参数通过submit提交到独立进程。提交任务必须是简单函数，进程并行不支持类方法、闭包等。
        函数参数和返回值必须兼容pickle序列化，进程间的通信需要
        '''
        future_result = pool.submit(calc, keep_stock_threshold, buy_change_threshold)
        # 当进程完成任务后，即cale运行结束后的回调函数
        future_result.add_done_callback(when_done)

print(sorted(res)[::-1][:10])
```

使用ProcessPoolExecutor实现多进程，具体的细节已经标注在代码中了

对于这个例子来说，使用多进程运行速度反而变慢了，因为要对进程进行创建、销毁、进程间通信等操作，这些操作都要消耗一定的资源，而进程的优势在本本例有没有显现，所以耗时更长了些

使用多线程，这里使用ThreadPoolExecutor来实现多线程，用法与多进程很相似

结果如下：

```bash
[(0.45400000000000007, 28, -0.06),
(0.381, 28, -0.07),
(0.36100000000000004, 26, -0.05),
(0.33300000000000013, 24, -0.06),
(0.3320000000000001, 18, -0.05),
(0.33199999999999996, 22, -0.06),
(0.32200000000000006, 16, -0.05),
(0.3159999999999999, 26, -0.06),
(0.301, 24, -0.07),
(0.298, 24, -0.05)]
```

使用多线程来提高运行速度，使用方式跟多进程非常相似

```python
from concurrent.futures.thread import ThreadPoolExecutor

res = []
def when_donw(r):
    res.append(r.result())
    
with ThreadPoolExecutor() as pool:
    for keep_stock_threshold, buy_change_threshold in itertools.product(keep_stock_list, buy_change_list):
        future_result = pool.submit(calc, keep_stock_threshold, buy_change_threshold)
        future_result.add_done_callback(when_donw)

print(sorted(res)[::-1][:10])
```

结果如下：

```bash
[(0.45400000000000007, 28, -0.06),
(0.3890000000000001, 10, -0.06), 
(0.381, 28, -0.07), 
(0.36100000000000004, 26, -0.05),
(0.33300000000000013, 24, -0.06),
(0.3320000000000001, 18, -0.05),
(0.33199999999999996, 22, -0.06),
(0.32200000000000006, 16, -0.05),
(0.3159999999999999, 26, -0.06),
(0.30499999999999994, 24, -0.07)]
```

观察多线程运行结果，可以发现它的结果与串行运行和多进程运行的结果不一致，**其原因在于clac()函数中的set_keep_stock_threshold方法和set_buy_change_threshold方法分别是类方法和静态方法，并非实例方法，在同一进程中的多个线程不断针对类变量设置参数，结果是错误的**

### 20

```python
with A_Class() as a:
    do_something
```

使用with作为关键字开头在python中成为上下文管理器，特点是：

+ 在进入上下文管理器定义的缩进模块后，会触发A_Class中定义的、\__enter\__()函数
+ 在结束上下文管理器定义的缩进模块后，会触发A_Class中定义的\__exit\__()函数

一般在\__enter\__()和\__exit\__()函数中定义相反的操作，如文件打开关闭，资源的创建和释放等

在线程锁threading.RLock中可以找到如下实现：

```python
def __enter__(self):
    self.acquire() #上锁
    
def __exit__(self, t, v, tb):
    self.release() #释放锁
```

### 21
使用编译库来提高性能

+ Numexpr可以快速计算数值，确定是局限性大
+ numba运行是动态编译python代码提高效率
+ Cython静态编译Python代码提高效率

接着尝试使用numba来提升性能效率

为了方便计时，写一个简单的计时装饰器

```python
import time

def run_time(func):
    def print_time(*args, **kwargs):
        t0 = time.perf_counter()
        res = func(*args, **kwargs)
        t1 = time.perf_counter()
        print('%s run time is (%s)'%(func.__name__,t1-t0))
        return res
    return print_time
```

?>time.perf_counter()方法专用于性能测试计时,返回性能计数器的值（以分秒为单位),即具有最高可用分辨率的时钟，以测量短持续时间。它包括在睡眠期间和系统范围内流逝的时间。返回值的参考点未定义，因此只有连续调用结果之间的差异有效。

接着放大最优参数组合的寻找范围

```python
keep_stock_list = range(1,504,1)
buy_change_list = [buy_change/100.0 for buy_change in range(-1,-100,-1)]
```

使用最简单的串行运行方式来查找最优参数组合

```python
@run_time
def do_single_task():
    res = []
    task_list = list(itertools.product(keep_stock_list, buy_change_list))
    for keep_stock_threshold, buy_change_threshold in task_list:
        res.append(calc(keep_stock_threshold, buy_change_threshold))
    return res

res = do_single_task()
print(sorted(res)[::-1][:5])
```

运行结果如下：

```bash
do_single_task run time is (47.27531293100037)

[(1.0210000000000001, 18, -0.02), 
(0.9280000000000004, 8, -0.02),
(0.9280000000000003, 20, -0.02), (0.9260000000000004, 13, -0.02), 
(0.9110000000000004, 57, -0.03)]
```

接着使用numba库，将方法动态编译，以提高执行效率

```python
import numba as nb

@run_time
@nb.jit
def do_single_task():
    res = []
    task_list = list(itertools.product(keep_stock_list, buy_change_list))
    for keep_stock_threshold, buy_change_threshold in task_list:
        res.append(calc(keep_stock_threshold, buy_change_threshold))
    return res

res = do_single_task()
print(sorted(res)[::-1][:5])
```

使用了numba下的jit方法，该方法可以像装饰器一样使用，需要注意的是，**在有多个装饰器装饰一个函数时，nb.jit装饰器需要在最内层**

不然会报如下错误

!>TypeError: not enough arguments: expected 2, got 0

运行结果如下：

```bash
do_single_task run time is (48.553204597999866)
[(1.0210000000000001, 18, -0.02), 
(0.9280000000000004, 8, -0.02), 
(0.9280000000000003, 20, -0.02), 
(0.9260000000000004, 13, -0.02), 
(0.9110000000000004, 57, -0.03)]
```

没错，书中使用numba库编译后运行结果会更快，但在我这里，使用了time.perf_counter()进行计时后，串行运行反而更快

?>在ipython中可以使用%time魔术方法来计算运行耗时

### 22
使用logging模块打印日志，优点就是可以指定每条打印语句的输出级别，logging级别如下：

+ debug 调试信息
+ info 输出信息
+ warning 警告信息
+ error 错误信息
+ critical 严重错误

通过logging.basicConfig()统一设置运行环境日志的输出级别，只要大于或等于设置级别的日志才能打印输出，低于这个级别的被忽略

### 23
有时遇到问题，会比较棘手，此时可以使用**python调试工具pdb让程序单步运行，随时查看运行状态**

```python
import pdb

def gen_buy_change_list():
    buy_change_list =  []
    for buy_change in range(-5,-16,-1):
        if buy_change == -10:
            #打断点，通过set_trace()
            pdb.set_trace()

        buy_change = buy_change/100
        buy_change_list.append(buy_change)
    raise RuntimeError('debug for pdb')
    return buy_change_list

try:
    _ = gen_buy_change_list()
except Exception:
   # 从捕获异常的地方开始调试，是经常使用的调试技巧
    pdb.set_trace()
```

pdb调试经常使用的

```bash
> /Users/ayuliao/Desktop/workplace/MathMind/test2.py(10)gen_buy_change_list()
-> buy_change = buy_change/100
(Pdb) l
  5         for buy_change in range(-5,-16,-1):
  6             if buy_change == -10:
  7                 #打断点，通过set_trace()
  8                 pdb.set_trace()
  9     
 10  ->         buy_change = buy_change/100
 11             buy_change_list.append(buy_change)
 12         raise RuntimeError('debug for pdb')
 13         return buy_change_list
 14     
 15     try:
(Pdb) n
> /Users/ayuliao/Desktop/workplace/MathMind/test2.py(11)gen_buy_change_list()
-> buy_change_list.append(buy_change)
(Pdb) s
> /Users/ayuliao/Desktop/workplace/MathMind/test2.py(5)gen_buy_change_list()
-> for buy_change in range(-5,-16,-1):
(Pdb) c
--Return--
> /Users/ayuliao/Desktop/workplace/MathMind/test2.py(16)<module>()->None
-> _ = gen_buy_change_list()
(Pdb) 
```

在金融量化领域，不建议使用过短的变量名、类名、函数名等，因为**试错成本太高，略显臃肿的名字可以规避很多问题，为了弥补这样的问题，在金融量化领域，列表推到、字典推到、高阶函数、lambda函数、三目表达式等技巧使用比较频繁。

## 量化工具--Numpy
Numpy是很多科学计算与工程库的基础库，Numpy就是量化数据分析领域的基础数组。

Numpy底层实现使用了C语言和Forran语言的机制分配内存，输出一个非常大且联系的并由**同类型数组组成的内存区域**

### 1
构建10000个元素的普通列表循环求每个元素的平方，使用%timeit对比效率

```python
In [1]: import numpy as np

In [2]: normal_list = range(10000)

In [3]: %timeit [i**2 for i in normal_list]
100 loops, best of 3: 2.34 ms per loop
```
运行时间是2.34ms

使用np.arange()构建列表，运行效率会提高几个数量级

```python
In [2]: import numpy as np

In [3]: np_list  = np.arange(10000)

In [4]: %timeit np_list**2
The slowest run took 296.86 times longer than the fastest. This could mean that an intermediate result is being cached.
100000 loops, best of 3: 5.74 µs per loop
```

这里运行时间是5.74us

Numpy数组和普通列表的操作方式也是不同的，**Numpy通过广播机制作用于每个内部元素**，是一种并行化执行的思想，普通的list则作用于整体

通过代码来看差别

```python
In [5]: np_list = np.ones(5) * 3

In [6]: print(np_list)
[ 3.  3.  3.  3.  3.]

In [7]: n_list = [1,1,1,1,1] * 3

In [8]: print(n_list)
[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
```

### 2
Numpy常见的初始化操作

```python
In [10]: np.zeros(100)
Out[10]:
array([ 0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,
        0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,
        0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,
        0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,
        0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,
        0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,
        0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,
        0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.])

In [11]: np.zeros((3,2))
Out[11]:
array([[ 0.,  0.],
       [ 0.,  0.],
       [ 0.,  0.]])

In [12]: np.ones((3,2))
Out[12]:
array([[ 1.,  1.],
       [ 1.,  1.],
       [ 1.,  1.]])

In [13]: np.empty((2,3,3)) #shape x=2,y=3,z=3的矩阵，值随机
Out[13]:
array([[[ -0.00000000e+000,  -0.00000000e+000,   5.43472210e-323],
        [  0.00000000e+000,   0.00000000e+000,   0.00000000e+000],
        [  0.00000000e+000,   0.00000000e+000,   0.00000000e+000]],

       [[  0.00000000e+000,   0.00000000e+000,   0.00000000e+000],
        [              nan,               nan,  -0.00000000e+000],
        [ -3.11109778e+231,  -1.97626258e-323,   2.00390436e+000]]])

In [14]: np.ones_like(np_list) # 初始化序列与np_list意义，值全为1的矩阵
Out[14]: array([ 1.,  1.,  1.,  1.,  1.])

In [15]: np.zeros_like(np_list) # 初始化shape与np_list一样，值全为0的矩阵
Out[15]: array([ 0.,  0.,  0.,  0.,  0.])

In [16]: np.eye(3) #对角线全为1的单位矩阵
Out[16]:
array([[ 1.,  0.,  0.],
       [ 0.,  1.,  0.],
       [ 0.,  0.,  1.]])
```

`linspace()`方法，生成等间隔的数字

在0~1之间等间隔生成10个元素的序列

```
In [17]: np.linspace(0,1,10)
Out[17]:
array([ 0.        ,  0.11111111,  0.22222222,  0.33333333,  0.44444444,
        0.55555556,  0.66666667,  0.77777778,  0.88888889,  1.        ])
```

### 3
生成服从正态分布的涨跌幅数据

```
In [19]: stock_cnt = 200 #200支股票

In [20]: view_days = 504 #504个交易日

# 生成服从正态分布：均值期望=0，标准差=1的序列
In [21]: stock_day_change = np.random.standard_normal((stock_cnt, view_days))

In [22]: print(stock_day_change.shape)
(200, 504)

# 打印第一只股票，前5个交易日的涨跌幅情况
In [23]: print(stock_day_change[0:1, :5])
[[ 0.63464531 -0.37501036  0.35212285  0.97899974 -1.24315714]]
```

输出结果为200行504列矩阵，每一行代表一只股票，每一列代表一个交易日。上面输出的`0.63464531`表示第一个交易日中价格上涨0.63464531

这里通过切片的方式来选择数据

```python
In [24]: stock_day_change[0:2,:5]
Out[24]:
array([[ 0.63464531, -0.37501036,  0.35212285,  0.97899974, -1.24315714],
       [ 0.86518928, -0.79618503,  0.1065087 , -1.28334236,  0.30882189]])
```

上面代码表示前两只股票前5个交易日的涨跌数据

因为**Numpy内部实现机制全部是引用操作，所以当不使用`copy()`的时候得出的结果将会丢失**

下面让numpy居中中首位数据交互一下

```python
In [24]: stock_day_change[0:2,:5]
Out[24]:
array([[ 0.63464531, -0.37501036,  0.35212285,  0.97899974, -1.24315714],
       [ 0.86518928, -0.79618503,  0.1065087 , -1.28334236,  0.30882189]])

In [25]: tmp = stock_day_change[0:2,0:5].copy()

In [26]: stock_day_change[-2:,-5:]
Out[26]:
array([[ 0.79976582, -0.24276089, -0.59893516, -0.03116983, -0.517955  ],
       [-1.22468834, -1.14144833, -0.55910722, -0.22906528, -0.8932027 ]])

In [27]: stock_day_change[:2,:5] = stock_day_change[-2:,-5:]

In [28]: stock_day_change[-2:,-5:] = tmp

In [29]: stock_day_change[:2,:5],stock_day_change[-2:,-5:]
Out[29]:
(array([[ 0.79976582, -0.24276089, -0.59893516, -0.03116983, -0.517955  ],
        [-1.22468834, -1.14144833, -0.55910722, -0.22906528, -0.8932027 ]]),
 array([[ 0.63464531, -0.37501036,  0.35212285,  0.97899974, -1.24315714],
        [ 0.86518928, -0.79618503,  0.1065087 , -1.28334236,  0.30882189]]))
```

### 4
数据类型转换的目的有时为了数据规整，有时可以进一步看出有价值的数据

下面通过`astype(int)`将涨跌数据转换为int，这样可以更加清晰的发现涨跌数据两端的极限值

```python
In [31]: print(stock_day_change[:2,:5])
[[ 0.79976582 -0.24276089 -0.59893516 -0.03116983 -0.517955  ]
 [-1.22468834 -1.14144833 -0.55910722 -0.22906528 -0.8932027 ]]

In [32]: stock_day_change[:2,:5].astype(int)
Out[32]:
array([[ 0,  0,  0,  0,  0],
       [-1, -1,  0,  0,  0]])
```

如果想要规整的float数据，可以使用`np.around()`函数

```python
In [33]: np.around(stock_day_change[:2,:5],2)
Out[33]:
array([[ 0.8 , -0.24, -0.6 , -0.03, -0.52],
       [-1.22, -1.14, -0.56, -0.23, -0.89]])
```

很多时候需要处理的数据会有缺失，Numpy中的np.nan表示缺失

```python
In [34]: tmp_test = stock_day_change[:2,:5].copy()

In [35]: tmp_test[0][0] = np.nan

In [36]: tmp_test
Out[36]:
array([[        nan, -0.24276089, -0.59893516, -0.03116983, -0.517955  ],
       [-1.22468834, -1.14144833, -0.55910722, -0.22906528, -0.8932027 ]])
```

可以使用`np.nan_to_num()`函数来用0填充nan，实质pandas中的dropna()和fillna()方法更适合处理nan

```python
In [37]: tmp_test= np.nan_to_num(tmp_test)

In [38]: tmp_test
Out[38]:
array([[ 0.        , -0.24276089, -0.59893516, -0.03116983, -0.517955  ],
       [-1.22468834, -1.14144833, -0.55910722, -0.22906528, -0.8932027 ]])
```

### 5
找出切片内涨跌幅值超过0.5的股票时

```python
In [40]: mask = stock_day_change[:2,:5] > 0.5

# 获得筛选结果，bool数组
In [41]: mask
Out[41]:
array([[ True, False, False, False, False],
       [False, False, False, False, False]], dtype=bool)
In [44]: tmp_test[mask]
#筛选出mask中值为True的值
Out[44]: array([ 0.79976582])
```

一般通过一句代码来实现上面示例

```python
# 切片中大于0.5的元素赋值为1
In [46]: tmp_test[tmp_test > 0.5] = 1

In [47]: tmp_test
Out[47]:
array([[ 1.        , -0.24276089, -0.59893516, -0.03116983, -0.517955  ],
       [-1.22468834, -1.14144833, -0.55910722, -0.22906528, -0.8932027 ]])
```

针对多重筛选条件，使用|、&完成复合的逻辑，**需要使用括号括起来**

```python
In [48]: tmp_test = stock_day_change[-2:,-5:]

In [49]: tmp_test
Out[49]:
array([[ 0.63464531, -0.37501036,  0.35212285,  0.97899974, -1.24315714],
       [ 0.86518928, -0.79618503,  0.1065087 , -1.28334236,  0.30882189]])

In [50]: tmp_test[(tmp_test > 1) | (tmp_test < -1)]
Out[50]: array([-1.24315714, -1.28334236])
```

### 6
Numpy提供了通用序列化函数，可以高效方便地处理序列

`np.all()`判断序列中所有元素是否全部都是true，对bool序列进行 与 操作

```python
# 判断切片中的数据是否都大于0，也就是是否都是上涨
In [52]: np.all(stock_day_change[:2,:5]>0)
Out[52]: False
```

`np.any()`判断序列中是否有元素为true，对bool序列进行 或 操作

```python
In [53]: np.any(stock_day_change[:2,:5]>0)
Out[53]: True
```

两个序列对应的元素**两两比较**，`maximum()`结果集取大，`minimum()`结果集取小

```python
In [54]: np.maximum(stock_day_change[:2,:5], stock_day_change[-2:,-5:])
Out[54]:
array([[ 0.79976582, -0.24276089,  0.35212285,  0.97899974, -0.517955  ],
       [ 0.86518928, -0.79618503,  0.1065087 , -0.22906528,  0.30882189]])
```

`np.unique()`函数可以让序列集中唯一且重复的值组成新的序列

```python
In [56]: change_int = stock_day_change[:2,:5].astype(int)

In [57]: change_int
Out[57]:
array([[ 0,  0,  0,  0,  0],
       [-1, -1,  0,  0,  0]])

In [58]: np.unique(change_int)
Out[58]: array([-1,  0])
```

`np.diff()`操作前后两个临近数值进行减法运行，默认情况下axis=1,axis表示操作轴向

```python
# axis默认为1，横向操作
In [60]: np.diff(stock_day_change[:2,:5])
Out[60]:
array([[-1.04252671, -0.35617428,  0.56776533, -0.48678517],
       [ 0.08324002,  0.58234111,  0.33004194, -0.66413742]])

In [61]: stock_day_change[:2,:5]
Out[61]:
array([[ 0.79976582, -0.24276089, -0.59893516, -0.03116983, -0.517955  ],
       [-1.22468834, -1.14144833, -0.55910722, -0.22906528, -0.8932027 ]])

In [62]: 0.79976582 + 0.24276089
Out[62]: 1.04252671
```

axis=0，则是纵向操作

```python
In [63]: np.diff(stock_day_change[:2,:5], axis=0)
Out[63]: array([[-2.02445417, -0.89868744,  0.03982794, -0.19789545, -0.3752477 ]])
```

### 7

`np.where()`函数在数据筛选、改造中有非常大的作用，其语法有点像三目运算符，第一个参数是条件表单式，如果成功走第二个参数，否则走第三个参数，**它返回的结果是一组序列**

```python
In [67]: tmp_test = stock_day_change[-2:,-5:]

In [68]: np.where(tmp_test > 0.5, 1, 0)
Out[68]:
array([[1, 0, 0, 1, 0],
       [1, 0, 0, 0, 0]])
```

如果数值大于0.5，则标记为1，否则保持不变

```python
In [69]: np.where(tmp_test > 0.5 , 1, tmp_test)
Out[69]:
array([[ 1.        , -0.37501036,  0.35212285,  1.        , -1.24315714],
       [ 1.        , -0.79618503,  0.1065087 , -1.28334236,  0.30882189]])
```

如果逻辑表达式为复合逻辑条件，则使用`np.logical_and()`和`np.logical_or()`函数，下面实现值大于0.5并小于1的赋值为1，否则赋值为0

```python
In [70]: np.where(np.logical_and(tmp_test > 0.5 ,tmp_test < 1), 1, 0)
Out[70]:
array([[1, 0, 0, 1, 0],
       [1, 0, 0, 0, 0]])
```

**np.where()函数在量化数据分析场景中用处巨大**

### 8
通过`np.save()`函数可以将Numpy序列持久化保持，**不需要添加文件后缀，save内部会自动生成`.npy`

```python
In [72]: np.save('./numpytest', stock_day_change)
```

使用`np.load()`函数即可加载，但需要加上`.npy`文件后缀

```python
In [74]: stock_day_change = np.load('./numpytest.npy')

In [75]: stock_day_change.shape
Out[75]: (200, 504)
```

### 9
Numpy带来了不止是并行化执行的思想，更有统计学上的很多方法的实现，如期望`np.mean()`、方差`np.var()`、标准差`np.std()`

```python
In [76]: sdcf = stock_day_change[:4,:4]

In [77]: sdcf
Out[77]:
array([[ 0.79976582, -0.24276089, -0.59893516, -0.03116983],
       [-1.22468834, -1.14144833, -0.55910722, -0.22906528],
       [-0.64717303,  0.00724261,  0.0042882 , -0.43108485],
       [ 0.14903309, -0.04658385,  0.78992598, -0.17589422]])

# axis=1横向对比，找出最大的值
In [78]: np.max(sdcf, axis=1)
Out[78]: array([ 0.79976582, -0.22906528,  0.00724261,  0.78992598])
```

其他统计方法

```python
# 最大跌幅
In [79]: np.min(sdcf, axis=1)
Out[79]: array([-0.59893516, -1.22468834, -0.64717303, -0.17589422])

#振幅幅度
In [80]: np.std(sdcf, axis=1)
Out[80]: array([ 0.51403239,  0.41243818,  0.28295815,  0.37113559])

#平均涨跌
In [81]: np.mean(sdcf, axis=1)
Out[81]: array([-0.01827501, -0.78857729, -0.26668177,  0.17912025])

#最大涨幅
In [82]: np.max(sdcf, axis=0)
Out[82]: array([ 0.79976582,  0.00724261,  0.78992598, -0.03116983])

#返回沿轴axis最大值的索引,最大涨幅股票
In [83]: np.argmax(sdcf, axis=0)
Out[83]: array([0, 2, 3, 0])

#返回沿轴axis最小值的索引,最大跌幅股票
In [85]: np.argmin(sdcf, axis=0)
Out[85]: array([1, 1, 0, 2])
```

?>Python标准库中的`max()`和`min()`与`np.max()`和`np.min()`是相同的，在数据量非常小的情况下，Numpy库执行的效率会更慢，所有不必所有操作都用Numpy

### 10

`numpy.random.normal(loc=0.0, scale=1.0, size=None)` 高斯分布

loc：float
    此概率分布的均值（对应着整个分布的中心centre）
scale：float
    此概率分布的标准差（对应于分布的宽度，scale越大越矮胖，scale越小，越瘦高）
size：int or tuple of ints
    输出的shape，默认为None，只输出一个值

期望是试验中每次可能的结果的概率乘以其结果的总和，反应一组数据平均取值大学，用于表示分布的中心位置

$$ meax(X) = \mu = \sum_{i=1}^n{p_i}{x_i} $$

方差是衡量一组数据离散程度的度量，概率论中方差用来度量数据和其期望直接的离散程度，方差越大，说明数据越离散

$$ Var(X) = \sigma^2 = mean((X-\mu^2)) $$

标准差为方差的算术平方根，标准差和变量的计算单位相同，所以比方差清晰，**很多时候在分析时使用更多的是标准差，方差与标准差都可以用于表示分布的离散程度**

$$ Sid(X) = \sigma $$

有a,b两个交易员，平均获利都是100元，但a交易者获利的获利稳定性不好，假设振幅50，即标准差为50

```python
In [86]: a_inverstor = np.random.normal(loc=100, scale=50, size=(100,1))

In [87]: b_inverstor = np.random.normal(loc=100, scale=20, size=(100,1))

#期望
In [88]: a_inverstor.mean()
Out[88]: 104.92527326730597

#标准差
In [89]: a_inverstor.std()
Out[89]: 49.462933488780024

#方差
In [90]: a_inverstor.var()
Out[90]: 2446.5817893154763
```

常会用到的np.random.randn(size)所谓标准正态分布（μ=0,σ=1），对应于np.random.normal(loc=0, scale=1, size)

```python
In [100]: import matplotlib.pyplot as plt

# 收益绘制曲线
In [101]: plt.plot(a_inverstor)
Out[101]: [<matplotlib.lines.Line2D at 0x112cfd198>]

# 水平直线 上线  期望值
In [102]: plt.axhline(a_inverstor.mean()+a_inverstor.std(), color='r')
Out[102]: <matplotlib.lines.Line2D at 0x112d02da0>

# 水平直线 均值期望线
In [103]: plt.axhline(a_inverstor.mean(), color='y')
Out[103]: <matplotlib.lines.Line2D at 0x112d23d68>

# 水平直线 下线
In [104]: plt.axhline(a_inverstor.mean() - a_inverstor.std(), color='g')
Out[104]: <matplotlib.lines.Line2D at 0x112d31ba8>

In [105]: plt.show()
```

绘制结果如下：

![](http://p6un02lk4.bkt.clouddn.com/20180710112131.png)

除了上述分布工具外，偏度（skew）与峰度（kurt）也经常使用

+ skew偏度，密度还是曲线尾部的相对长度
+ kurt峰度，反映频数分布曲线顶端尖峭或扁平程度的指标

### 11

正态分布又被称为高斯分布，因为高斯在1809年用该分布来预测星体的位置，正态曲线成钟型，又被称为钟型曲线

正态分布特点：

+ 数据的标准差越大，数据分布离散程度越大
+ 数据的期望位于曲线的对称轴中心

```python
In [106]: import scipy.stats as scs
# 均值期望
In [107]: stock_mean = stock_day_change[0].mean()
# 标准差
In [108]: stock_std = stock_day_change[0].std()

In [109]: stock_mean
Out[109]: 0.021854761436593158

In [110]: stock_std
Out[110]: 1.0260375550650687

# 绘制股票的直方图
In [111]: plt.hist(stock_day_change[0], bins=50, normed=True)
Out[111]:
(array([ 0.01505249,  0.01505249,  0.        ,  0.03010498,  0.03010498,
         0.04515748,  0.04515748,  0.03010498,  0.10536744,  0.07526246,
         0.12041994,  0.12041994,  0.16557741,  0.1806299 ,  0.24083987,
         0.22578738,  0.1956824 ,  0.31610233,  0.40641728,  0.45157476,
         0.3763123 ,  0.36125981,  0.3763123 ,  0.51178473,  0.48167974,
         0.31610233,  0.31610233,  0.27094486,  0.28599735,  0.15052492,
         0.22578738,  0.21073489,  0.12041994,  0.09031495,  0.1806299 ,
         0.15052492,  0.07526246,  0.03010498,  0.06020997,  0.09031495,
         0.        ,  0.01505249,  0.06020997,  0.        ,  0.        ,
         0.        ,  0.        ,  0.        ,  0.        ,  0.01505249]),
 array([-2.96694602, -2.83513216, -2.70331831, -2.57150445, -2.4396906 ,
        -2.30787675, -2.17606289, -2.04424904, -1.91243518, -1.78062133,
        -1.64880747, -1.51699362, -1.38517977, -1.25336591, -1.12155206,
        -0.9897382 , -0.85792435, -0.72611049, -0.59429664, -0.46248278,
        -0.33066893, -0.19885508, -0.06704122,  0.06477263,  0.19658649,
         0.32840034,  0.4602142 ,  0.59202805,  0.7238419 ,  0.85565576,
         0.98746961,  1.11928347,  1.25109732,  1.38291118,  1.51472503,
         1.64653889,  1.77835274,  1.91016659,  2.04198045,  2.1737943 ,
         2.30560816,  2.43742201,  2.56923587,  2.70104972,  2.83286357,
         2.96467743,  3.09649128,  3.22830514,  3.36011899,  3.49193285,
         3.6237467 ]),
 <a list of 50 Patch objects>)

#linsapce从股票0最小值-->最大值生成数据
In [112]: fit_linspace = np.linspace(stock_day_change[0].min(), stock_day_change[0].max())

#概率密度函数（PDF，probability density function)又均值
# 方差来描述曲线使用 scripy.stats.norm.pdf生成拟合曲线
In [113]: pdf = scs.norm(stock_mean, stock_std).pdf(fit_linspace)

In [114]: plt.plot(fit_linspace, pdf, lw=2, c='r')
Out[114]: [<matplotlib.lines.Line2D at 0x1174223c8>]

In [115]: plt.show()
```
pdf()函数在统计学中称为概率密度函数，是指在某个确定的取值点附近的可能性的函数，将概率值分配给各个事件，得到事件的概率分布，让时间数值化

![](http://p6un02lk4.bkt.clouddn.com/20180710112911.png)

**统计套利中均值回复策略的理论依据为价格将围绕着价值上下波动**，正态分布最大的特点即为它的数据会围绕某个期望均值附近上下摆动，摆动幅度为数据的标准差。

### 12
使用正态分布实现简单的量化策略

继续使用之前生成的200支股票504天的服从正态分布的涨跌数据，保留后50天的最近数据作为策略的验证数据，并统计前454天的跌幅最大的3只股票，如果在第454天买入这3只股票会如何？

```python
In [1]: import numpy as np

In [2]: stock_day_change = np.random.standard_normal((200, 504))

In [3]: stock_day_change_test = stock_day_change[:200,0:504-50]

# 打印出前454跌幅最大的三支，总跌幅通过np.sum计算，np.sort对结果排序
In [4]: np.sort(np.sum(stock_day_change_test, axis = 1))[:3]
Out[4]: array([-62.70181139, -52.01698315, -45.03146593])

# 使用np.argsort针对股票跌幅进行排序，返回序号，即符合买入条件的股票序号
In [5]: stock_lower_array = np.argsort(np.sum(stock_day_change_test,axis=1))[:3]

In [6]: stock_lower_array
Out[6]: array([124, 129, 119])
```

```python
import numpy as np
import matplotlib.pyplot as plt

stock_cnt = 200
view_days = 504
keep_days = 50

# 建立符合正态分布的涨跌数据，均值期望＝0，标准差＝1的序列
stock_day_change = np.random.standard_normal((stock_cnt, view_days))
stock_day_change_test = stock_day_change[:stock_cnt,0:view_days-keep_days]
# 使用np.argsort针对股票跌幅进行排序，返回序号，即符合买入条件的股票序号
stock_lower_array = np.argsort(np.sum(stock_day_change_test,axis=1))[:3]
def show_buy_lower(stock_ind):
    # 设置一个一行两列的可视化图表
    _, axs = plt.subplots(nrows=1, ncols=2, figsize=(10, 4))
    # 绘制前454天股票走势图，np.arange()在给定范围内获取均值，np.cumsum()：序列连续求和，涨跌数据相加，就是股票的涨幅的走势
    axs[0].plot(np.arange(0, view_days - keep_days),
                stock_day_change_test[stock_ind].cumsum())
    # 从第454天开始到504天的股票走势
    cs_buy = stock_day_change[stock_ind][view_days - keep_days:view_days].cumsum()
    # 绘制从第454天到504天股票走势图
    axs[1].plot(np.arange(view_days - keep_days, view_days), cs_buy)
    # 返回从第454天开始到第504天计算盈亏的盈亏序列的最后一个值，通过一系列涨幅数据相加，最后一个数就是最终的涨跌值
    return cs_buy[-1]

# 最后输出的盈亏比例
profit = 0

# 跌幅最大的三支遍历序号
for stock_ind in stock_lower_array:
    # profit即三支股票从第454天买入开始计算，直到最后一天的盈亏比例
    profit += show_buy_lower(stock_ind)

print('买入第 {} 支股票，从第454个交易日开始持有盈亏:{:.2f}%'.format(
    stock_lower_array, profit))

plt.show()
```

输出结果和图像

买入第 [183  46 149] 支股票，从第454个交易日开始持有盈亏:5.62%

![](http://p6un02lk4.bkt.clouddn.com/20180711112637.png)

我们选择了前454中跌幅最大的3只股票的理论依旧的事正态分布理论，这3只股票后期涨跌的分布一定是涨的概率大于跌的概率

ayuliao:实际运行多次，可以发现，盈亏结果优势是负数，原因可能是，虽然涨的概率大于跌的概率，但是跌的幅度可能大于涨的幅度

早期华尔街很多策略算法认为市场的涨跌服从正态分布：

+ 一方面，很多统计数据确实显示正态分布存在于很多金融市场
+ 另一方面，编写符合正态分布规律的算法更加简单

现实生活汇总，由于人的参与以及很多突发事件，导致市场不可能绝对服从正态分布，导致“肥尾”

### 13
伯努利分布下，随机变量只有两个可能的取值，即1和0，如果随机变量取值1的概率为怕，则随机变量取值0的概率为1-p

Numpy中使用`numpy.random.binomial(1,p)`获取1的概率为p的前提下，生成的随机变量。如果p=0.5，那么久类似投硬币的结果

### 14
在交易中，交易者容易处于不利地位的，不利的情况就是需要交手续费，如果你随机乱买股票，在足够多的本钱的情况下，交易足够多的次数，最后的结果一定是符合正态分布的。但如果每次要多交1%的手续费，那情况就大不一样了。

实现函数`casino()`:假设100个赌徒，每个赌徒都有1000000元，并且每个人都想在赌场玩1000万次，在不同的胜率、赔率和手续费下`casino()`函数返回总体统计结果


```python
import numpy as np
import matplotlib.pyplot as plt

gamblers = 100 #100个赌徒

def casino(win_rate, win_once=1, loss_once=1, commission=0.01):
    my_money = 1000000
    play_cnt = 10000000
    commission = commission
    for _ in np.arange(0, play_cnt):
        # win_rate赢钱概率,根据伯努利分布根据win_rate获取输赢
        w = np.random.binomial(1, win_rate)
        if w:
            # 赢钱，win_once赢钱数
            my_money += win_once
        else:
            my_money -= loss_once

        my_money -= commission #手续费
        # 没钱就无法继续游戏
        if my_money <=0:
            break
    return my_money

# 没有老千，胜率为0.5，不收手续费，赔率是1，赔率等于 win_once/loss_once
heaven_moneys = [casino(0.5, commission=0) for _ in np.arange(0, gamblers)]
plt.title('heaven_moneys')
plt.hist(heaven_moneys, bins=30)
plt.show()

# 有老千，胜率为0.4，不收手续费，赔率是1
cheat_moneys = [casino(0.4, commission=0) for _ in np.arange(0, gamblers)]
plt.title('cheat_moneys')
plt.hist(cheat_moneys, bins=30)
plt.show()

# 没有老千，胜率为0.5，手续费0.01，赔率是1
commission_moneys = [casino(0.5, commission=0.01) for _ in np.arange(0, gamblers)]
plt.title('commission_moneys')
plt.hist(commission_moneys, bins=30)
plt.show()
```

没有老千，胜率为0.5，不收手续费，赔率是1的情况下，大家都活下来了，每个人亏的和赚的最后都不太多

![](http://p6un02lk4.bkt.clouddn.com/20180711132555.png)

有老千，胜率为0.4，不收手续费，赔率是1的情况下，所有人的钱都归0了

![](http://p6un02lk4.bkt.clouddn.com/20180711125800.png)

有老千，胜率为0.5，手续费0.01，赔率是1的情况下，没有人赚钱，都是亏钱(手上的金额都小于初始化金额1000000)，如果玩的次数在加大一个数量级，**最后的结果也是全部人的钱都归0**

![](http://p6un02lk4.bkt.clouddn.com/20180711135801.png)

如果将游戏次数设置为100万次，情况就有所不同

![](http://p6un02lk4.bkt.clouddn.com/20180711121623.png)
![](http://p6un02lk4.bkt.clouddn.com/20180711122006.png)
![](http://p6un02lk4.bkt.clouddn.com/20180711142937.png)

只有当游戏的次数足够多时，才能发现规律

我们不能期待自己有**老千**的能力，也就是赢的概率无法提高，那为了赚钱，只能将赔率提高了，**赔率=每次赢钱数/每次输钱数**

下面我们将win_once提高到1.02，loss_once降低到0.98，这样赔率就是 1.02/0.98 = 1.040816，让100个赌徒进入这个非均衡赔率的赌场

结果如图：

![](http://p6un02lk4.bkt.clouddn.com/20180711150021.png)

发现每个人都赚了钱（金额都比1000000多），这不合理，凭什么你每次多赢输少呢？（赢钱转1.02元，输钱数0.98元），为了让其合理，应该要降低赢的概率，这样做其实就是每本交易的书都会提到止损策略。

这些策略的核心思想就是让赚钱的股票多赚钱，让亏损的股票少亏损，这样的结果就是亏损的次数比赢钱的次数多，但每次赢的钱比每次亏的钱多，也就是高止盈位，低止损位。

使用上面的止盈止损策略会降低胜率，所以不能平白无故提高win_once，对应的需要降低win_rate，将win_rate降低为0.45，即胜率为0.45，win_once和loss_once都不变动。

运行结果如下：

![](http://p6un02lk4.bkt.clouddn.com/20180711152815.png)

有变回回了有赢有输的情况，也就是`heaven_moneys`的情况

**大多数交易者的胜率非常高，但他们的账号最终都是亏损的，交易中最虚幻的就是胜率，但大多数人追求的反而是胜率，不关注盈亏比等其他重要因素，量化交易的核心依旧是交易，交易的基本法则依旧适用**

## 量化工具--pandas

### 1
pands是金融数据分析、量化交易等领域的重要工具，pandas的两大主要工具DataFrame和Series的框架设计与API参考方向都来源与统计分析语言R。

如果你有几GB，甚至1-2TB，使用pandas来处理数据依旧是最好的选择，对于更大的数据，就需要动用Hadoop、Spark了，它们的优势在于集群的云端数据处理

### 2
在前面使用Numpy中，使用了`np.argmax()`来纵向寻找哪一只股票在某个交易日涨幅最大，`np.argmax()`方法返回的也只是一个数字序列

而使用pandas就简单很多了，首先构建一个DataFrame对象，只传入Numpy对象stock_day_cahgne，使用`head()`方法显示生成的表格对象的前5行数据

```python
import numpy as np
import pandas as pd

stock_day_change = np.random.standard_normal((200, 504))
pd.DataFrame(stock_day_change).head()
```

![](http://p6un02lk4.bkt.clouddn.com/20180711132856.png)

?>pandas是在numpy基础上构建的，所以Numpy的很多实用方法以及Numpy的很多通用函数都可以直接在pandas的对象上直接使用，如切片

```python
pd.DataFrame(stock_day_change)[:5]
```
效果与上面的一样

![](http://p6un02lk4.bkt.clouddn.com/20180711133044.png)

使用Numpy数组直接构建DataFrame对象会默认将行列索引加上数据序号
但这样行列索引名称的意义没有什么作用，下面就让行列索引产生实质的意义

使用股票0、股票1作为股票的行索引

```python
#股票0 --> 股票stock_day_change.shape[0]
stock_symbols = ['股票'+str(x) for x in range(stock_day_change.shape[0])]
# 通过构造直接设置inde参数，head(2)就显示两行
pd.DataFrame(stock_day_change, index=stock_symbols).head(2)
```

![](http://p6un02lk4.bkt.clouddn.com/20180711133725.png)

使用`pd.date_range()`函数生成一组连续的时间序列

```python
days = pd.date_range('2017-1-1', 
                     periods=stock_day_change.shape[1], freq='1d')
stock_symbols = ['股票' + str(x) 
                 for x in range(stock_day_change.shape[0])]
# 行通过index指定索引，列通过columns指定索引
df = pd.DataFrame(stock_day_change, index=stock_symbols, columns=days)
df.head(2)
```

![](http://p6un02lk4.bkt.clouddn.com/20180711145022.png)

### 3

量化交易中，最常见的数据类型就是金融时间序列，对于时间序列的处理，pandas有非常友好的方法来分析挖掘数据

下面代码首先将数据df(上面刚刚生成的数据)做个**转置**，得到行索引为时间，列索引为股票代码的金融时间序列

```python
df = df.T #转置
df.head() #获得数据前5行
```

![](http://p6un02lk4.bkt.clouddn.com/20180711145849.png)

```python
# 对df重新采样，以21天为周期，对21天内的时间求平局来重塑数据
df_20 = df.resample('21D').mean()
df_20.head()
```

![](http://p6un02lk4.bkt.clouddn.com/20180711145806.png)


```python
#通过 df['股票0']获得股票数据，返回变量类型是series
df_stock0 = df['股票0']
print(type(df_stock0))
df_stock0.head()
```

结果如下：

```
<class 'pandas.core.series.Series'>
2017-01-01   -1.194748
2017-01-02    1.387978
2017-01-03    1.547652
2017-01-04    1.345085
2017-01-05    1.015103
Freq: D, Name: 股票0, dtype: float64
```

series是pandas中非常重要的类，可以将series简单理解为只有一列数据的DataFrame对象，它们之间大多数函数都可以通用，使用方法也很类似，如head()函数，打印出Series的前5行数据

?>jupyter显示matplotlib图片，在绘图前，使用%matplotlib inline

```python
%matplotlib inline
df_stock0.cumsum().plot()
```

![](http://p6un02lk4.bkt.clouddn.com/20180711151649.png)

?>pandas在基于封装numpy的数据操作上还封装了matplotlib，使用它可以通过很简短的代码实现复杂的任务

### 4
之前使用`resample()`函数重采样，所有股票网站都提供了日k线、周k线、月k线等周期数据，但最原始的数据只有日k线数据，下面通过重采样实现周k线、月k线构建

```python
#cumsum()序列连续求和
df_stock0_5 = df_stock0.cumsum().resample('5D').ohlc()
df_stock0_20 = df_stock0.cumsum().resample('21D').ohlc()
df_stock0_5.head()
```

![](http://p6un02lk4.bkt.clouddn.com/20180711152415.png)

将数据的高开低收k线图绘制出来，也就是成功的从日线级别的数据重写构建出周线数据，并且可视化，这里要使用abu系统中的代码，我将其单独抽取出来

```
import matplotlib.pyplot as plt


"""不画量只画价格"""
g_only_draw_price = False

# TODO 可设置的红涨绿跌，还是绿涨红跌
__colorup__ = "red"
__colordown__ = "green"

def _do_plot_candle(date, p_open, high, low, close, volume, view_index, symbol, day_sum, minute):
    """
    绘制不可交互的k线图
    param date: 融时间序列交易日时间，pd.DataFrame.index对象
    :param p_open: 金融时间序列开盘价格序列，np.array对象
    :param high: 金融时间序列最高价格序列，np.array对象
    :param low: 金融时间序列最低价格序列，np.array对象
    :param close: 金融时间序列收盘价格序列，np.array对象
    :param volume: 金融时间序列成交量序列，np.array对象
    :param symbol: symbol str对象
    :param day_sum: 端线图 matplotlib的版本有些有bug显示不对
    :param minute: 是否是绘制分钟k线图
    """

    # 需要内部import不然每次import abupy都有warning，特别是子进程很烦人
    try:
        # noinspection PyUnresolvedReferences, PyDeprecation
        import matplotlib.finance as mpf
    except ImportError:
        # 2.2 才会有
        # noinspection PyUnresolvedReferences, PyDeprecation
        import matplotlib.mpl_finance as mpf

    if not g_only_draw_price:
        # 成交量，价格都绘制
        # noinspection PyTypeChecker
        fig, (ax1, ax2) = plt.subplots(2, sharex=True, figsize=(20, 12))
    else:
        # 只绘制价格
        fig, ax1 = plt.subplots(figsize=(6, 6))
    if day_sum:
        # 端线图绘制
        qutotes = []
        for index, (d, o, c, l, h) in enumerate(zip(date, p_open, close, low, high)):
            d = index if minute else mpf.date2num(d)
            val = (d, o, c, l, h)
            qutotes.append(val)
        # plot_day_summary_oclh接口，与mpf.candlestick_ochl不同，即数据顺序为开收低高
        mpf.plot_day_summary_oclh(ax1, qutotes, ticksize=5, colorup=__colorup__, colordown=__colordown__)
    else:
        # k线图绘制
        qutotes = []
        for index, (d, o, c, h, l) in enumerate(zip(date, p_open, close, high, low)):
            d = index if minute else mpf.date2num(d)
            val = (d, o, c, h, l)
            qutotes.append(val)
        # mpf.candlestick_ochl即数据顺序为开收高低
        mpf.candlestick_ochl(ax1, qutotes, width=0.6, colorup=__colorup__, colordown=__colordown__)

    if not g_only_draw_price:
        # 开始绘制成交量
        ax1.set_title(symbol)
        ax1.set_ylabel('ochl')
        ax1.grid(True)
        if not minute:
            ax1.xaxis_date()
        if view_index is not None:
            # 开始绘制买入交易日，卖出交易日，重点突出的点位
            e_list = date.tolist()
            # itertools.cycle循环使用备选的颜色
            for v, csColor in zip(view_index, itertools.cycle(K_PLT_MAP_STYLE)):
                try:
                    v_ind = e_list.index(v)
                except Exception as e:
                    # logging.exception(e)
                    logging.debug(e)
                    # 向前倒一个
                    v_ind = len(close) - 1
                label = symbol + ': ' + str(date[v_ind])
                ax1.plot(v, close[v_ind], 'ro', markersize=12, markeredgewidth=4.5,
                         markerfacecolor='None', markeredgecolor=csColor, label=label)

                # 因为candlestick_ochl 不能label了，所以使用下面的显示文字
                # noinspection PyUnboundLocalVariable
                ax2.plot(v, 0, 'ro', markersize=12, markeredgewidth=0.5,
                         markerfacecolor='None', markeredgecolor=csColor, label=label)
            plt.legend(loc='best')

        # 成交量柱子颜色，收盘价格 > 开盘，即红色
        # noinspection PyTypeChecker
        bar_red = np.where(close >= p_open, volume, 0)
        # 成交量柱子颜色，开盘价格 > 收盘。即绿色
        # noinspection PyTypeChecker
        bar_green = np.where(p_open > close, volume, 0)

        date = date if not minute else np.arange(0, len(date))
        ax2.bar(date, bar_red, facecolor=__colorup__)
        ax2.bar(date, bar_green, facecolor=__colordown__)

        ax2.set_ylabel('volume')
        ax2.grid(True)
        ax2.autoscale_view()
        plt.setp(plt.gca().get_xticklabels(), rotation=30)
    else:
        ax1.grid(False)

    plt.show()
      
        

_do_plot_candle(df_stock0_5.index,                                 df_stock0_5['open'].values,                                   df_stock0_5['high'].values,                                   df_stock0_5['low'].values,                                  df_stock0_5['close'].values,                                np.random.random(len(df_stock0_5)),
 None, 'stock',day_sum=False,minute=False)
```    

绘制结果如下：

![](http://p6un02lk4.bkt.clouddn.com/20180711155141.png)

图是通过matplotlib绘制的，这里的成交量volume是随机生成的数据

### 5

通过真实数据来使用pandas，书中使用abu系统中的方法获取沙箱里特斯拉2年内的股票数据，这里依旧通过tushare来获得汇川技术2016-2018这两年内的股票数据

```python
import tushare as ts
hc_data = ts.get_k_data('300124',start='2016-01-01', end='2018-01-01')
print(type(hc_data))
hc_data.tail()
```

tushare获取数据的类型就是pandas的DataFrame，使用tail()可以获得DataFrame最后5行数据

![](http://p6un02lk4.bkt.clouddn.com/20180711160535.png)

通过pandas的plot()函数绘制出汇川技术这支股票在2016年到2018年这两年间的大致情况

![](http://p6un02lk4.bkt.clouddn.com/20180711160736.png)

通过pandas的DataFrame对象总览数据的函数info()的用途是查看数据是否有缺失、以及各个子数据的数据类型


