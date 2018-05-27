# 《Redis入门指南》要点笔记

## 简介

### 1
Redis是开源、高性能、基于键值对的缓存与存储系统，同时Redis还用于消息队列、任务队列

### 2
Redis支持如下数据类型：

+ 字符串类型（string）
+ 散列类型（hash）
+ 列表类型（list）
+ 集合类型（set）
+ 有序集合类型（sorted set）

Redis字典结构的存储方式和对多键值数据类型的支持使得开发者可以将程序中的数据直接映射到Redis中，数据在Redis中的存储形式和其他在程序中的存储方式非常相近

### 3
Redis数据库中的所有数据都存储在内存中。Redis可以在一秒内读写超过10万个键值

同时Redis支持持久化，可以将内存中的数据**异步**写入到硬盘中，不影响继续提供服务

### 4
Redis与Memcached优劣：

性能上： Redis单线程模式，Memcached支持多线程，理论上Memcached性能更高，但Redis性能以及足够优秀了

Redis 3.0推出标志着Memcached几乎所有功能成为了Redis的子集

### 5
作为缓存系统，Redis可以限定数据占用最大内存空间，在数据到达空间限制后，可以按照一定的规则自动淘汰不需要的键

通过Redis很容易实现一个高性能的优先级队列，支持阻塞式读取

## 准备

### 1
Mac下安装Redis：

1.安装Homebrew包管理工具

2.通过Homebrew安装Redis

```bash
brew install redis
```

Redis官方不支持Windows，2011年微软向Redis提交一个补丁，以使Redis可以在windows下编译运行，被Redis作者拒绝

微软自己Fork了一份Redis代码，开发兼容Windows的Redis

### 2
控制Redis几个关键命令

+ redis-server          Redis服务器
+ redis-cli             Redis命令行客户端
+ redis-benchmark       Redis性能测试工具
+ redis-check-aof       AOF文件修复工具
+ redis-check-dump      RDB文件检查工具
+ redis-sentinel        Sentinel服务器（2.8版后才有）

### 3
启动Redis：

```bash
redis-server
```

启动Redis时指定端口号，默认是6379端口

```bash
redis-server --port 2333
```

### 4
通过脚本的形式启动Redis

Linux系统可以通过初始化脚本启动Redis，使得Redis能随系统自动运行，**在生成环境中，推荐使用脚本的方式启动Redis**

在Redis源代码目录的utils下有个名叫redis_init_script的文件，就是Redis默认提供给我们的初始化脚本，代码如：

```
#!/bin/sh
#
# Simple Redis init.d script conceived to work on Linux systems
# as it does use of the /proc filesystem.

REDISPORT=6379
EXEC=/usr/local/bin/redis-server
CLIEXEC=/usr/local/bin/redis-cli

PIDFILE=/var/run/redis_${REDISPORT}.pid
CONF="/etc/redis/${REDISPORT}.conf"

case "$1" in
    start)
        if [ -f $PIDFILE ]
        then
                echo "$PIDFILE exists, process is already running or crashed"
        else
                echo "Starting Redis server..."
                $EXEC $CONF
        fi
        ;;
    stop)
        if [ ! -f $PIDFILE ]
        then
                echo "$PIDFILE does not exist, process is not running"
        else
                PID=$(cat $PIDFILE)
                echo "Stopping ..."
                $CLIEXEC -p $REDISPORT shutdown
                while [ -x /proc/${PID} ]
                do
                    echo "Waiting for Redis to shutdown ..."
                    sleep 1
                done
                echo "Redis stopped"
        fi
        ;;
    *)
        echo "Please use start or stop as first argument"
        ;;
esac
```

要使用该脚本，需要进行下面操作

1.将初始化脚本复制到 /etc/init.d 目录下，文件名为redis_端口号，然后修改第六行REDISPORT变量的值为相同的端口号

2.建立相应的文件夹

+ /etc/redis            存放Redis的配置文件
+ /var/redis/端口号      存放Redis的持久化文件

3.修改配置文件，将配置文件模板复制到 /etc/redis 目录中，以端口号命名，如：6379.conf

参数|值|说明
---|---|---
daemonize|yes|使Redis以守护进程模式运行
pidfile|/var/run/redis_端口号.pid|设置Redis的PID文件位置
port|端口号|设置Redis监听端口号
dir|/var/redis/端口号|设置持久化文件存放位置

4.使用 /etc/init.d/redis_端口号 start 来启动Redis，然后执行下面命令让Redis可以随系统自动启动

```bash
sudo update-rc.d redis_端口号 defaults
```

### 5
停止Redis

考虑到Redis有可能正在将内存中的数据同步到硬盘中，强行终止Redis进程可能会到时数据丢失

正确停止Redis的方式是向Redis服务器发送SHUTDOWN命令

```bash
redis-cli SHUTDOWN
```

当Redis收到SHUTDOWN命令后，会先断开所有客户端的链接，然后根据配置执行持久化，完成这些后Redis才会退出

Redis可以妥善处理 SIGTERM 信号，所以使用kill Redis进程的PID也可以正常结束Redis，与 SHUTDOWN 命令效果一致

### 6
redis-cli向Redis发送命令有两种方式

1.将命令作为redis-cli的参数执行，如：

```bash
redis-cli SHUTDOWN
```

redis-cli会自动按照默认配置（服务器127.0.0.1，端口号6379）链接Redis，通过-h和-p参数可以自定义地址和端口号

```bash
redis-cli -h 127.0.0.1 -p 6379
```

Redis提供PING命令，用于测试客户端与Redis连接是否正常，连接正常会收到PONG

```bash
redis-cli PING
PONG
```

2.第二种方式，不附带惨运行redis-cli，直接进入交互模式

```bash
redis-cli
redis 127.0.0.1> PING
PONG
```

### 7
命令有5种类型的返回值

1.状态回复，直接显示状态信息，如：

```bash
127.0.0.1:6379[1]> ping
PONG
127.0.0.1:6379[1]> set b '123'
OK
```

2.错误回复，当使用的命令不存在或格式有问题时，Redis会返回错误回复，如：

```bash
127.0.0.1:6379[1]> ayuliao
(error) ERR unknown command 'ayuliao'
```

3.整数回复，Redis中虽然没有整数类型，但却提供了用于整数操作的命令，如INCR自增操作：

```bash
127.0.0.1:6379[1]> incr a
(integer) 1
127.0.0.1:6379[1]> incr a
(integer) 2
```

4.字符串回复

```bash
127.0.0.1:6379[1]> get a
"2"
127.0.0.1:6379[1]> get m
(nil)
```

5.多字符串回复

```bash
127.0.0.1:6379[1]> keys *
1) "b"
2) "a"
```

### 8

开启Redis指定使用相应的配置文件

```bash
redis-server /etc/redis.conf
```

启动参数传递同名配置选项会覆盖配置文件中相应的参数

```bash
redis-server /etc/redis.conf --loglevel warning
```

Redis提供了配置文件的模板 redis.conf，在源代码的根目录中

Redis可以通过 CONFIG SET 命令在不需要重写Redis的情况下动态修改Redis配置，并不是所有的配置都可以通过 CONFIG SET 命令进行修改的

### 9
一个Redis实例提供多个用来存储数据的字典，客户端可以指定将数据存储到对应的字典中，类似于一个MySQL实例中可以有创建多个数据库一样

每个数据库对外都是以一个从0开始的递增数字命名，Redis默认支持16个数据库，通过配置 databases 参数来修改这个数字，客户端与Redis建立连接后会自动选择0号数据库，可以通过SELECT命令更换数据库，如选择1号数据库

```
127.0.0.1:6379[1]> select 1
OK
```

### 10
Redis不支持自定义数据库名字，每个数据库都以编号命名，开发者必须自己记录哪些数据库存储了哪些数据

Redis不支持为每个数据库设置不同的访问密码，所以一个客户端要么可以访问全部数据库，要么一个数据库也访问不了

Redis数据库之间不是完全隔离，比如 FLUSHALL 命令可以清空一个Redis实例中所有数据库中的数据

Redis中的数据库其实更像一个命名空间，而不适宜存储不同应用程序的数据，比如可以使用0号数据库存储A应用生成环境的数据，使用1号数据库存储测试环境的数据，但不宜使用0号数据库存储A应用中的数据而用1号数据库存储B应用中的数据

不同的应用应该使用不同的Redis实例进行数据存储，因为Redis非常轻量级，一个空Redis实例占用内存只有1MB左右，不用担心多个Redis实例会额外占用很多内存

## 入门

### 1

获得符合规则的键名列表

```bash
KEYS pattern
```

pattern支持glob风格通配符

### 2

判断一个键是否存在

```bash
EXISTS key
```

键存在返回1，否则返回0

### 3

删除键

```bash
DEL key [key...]
```

返回值表示删除的键的个数

DEL命令参数不支持通配符，但可以结合Linux股电脑和xargs命令实现删除所有符合规则的键，如要删除所有以 user: 开头的键，可以执行如下命令

```bash
redis-cli KEYS "user:*" | xargs redis-cli DEL
```

由于DEL命令支持多个键作为参数，所有还可以执行

```bash
redis-cli DEL 'redis-cli KEYS "user:*"'
```

第二种方式性能更好

### 4
获取键值的数据类型

```bash
127.0.0.1:6379[1]> set name ayuliao
OK
127.0.0.1:6379[1]> type name
string
```

可以使用TYPE命令来获得键值的数据类型，返回值可能是string、hash、list、set、zset(有序集合类型)

### 5
字符串类型的赋值与取值

```bash
SET key value
GET key
```

### 6
当存储的字符串是整数形式时，可以使用INCR命令，其作用是让当前键值递增，并返回递增后的值

```bash
127.0.0.1:6379[1]> set num 1
OK
127.0.0.1:6379[1]> incr num
(integer) 2
```

当要操作的键不存在时，会默认键值是0

如果INCR命令操作的不是整数型字符串，就会报错

当然你可以通过GET和SET方法自己实现一个INCR函数，伪代码如：

```python
def incr(key):
    value = GET key
    if not value:
        value = 0
    value = value + 1
    SET key,value
    return value
```

如果Redis同时只连接了一个客户端，上面的代码没有任何问题，但是如果Redis同一时间被多个客户端连接就可能会出现竞态条件

?>竞态条件：多个客户端同时操作一个对象，可能让该对象的值混乱，常见的做法是给该对象加锁，只允许一个客户端对其操作，操作完后释放锁，其他客户端才能接着操作

当Redis有多个客户端连接的情况下，上面那段自定义代码就会出现竞态条件，但INCR命令却不会，因为它是原子操作

?>原子操作表示不可再分的操作

### 7
Redis命名最好通过下面规范来命名：

?> 对象类型：对象ID：对象属性

如使用这个user : 1 : friends键名来存储ID为1的用户好友列表

如果有多个键名中要通过多个单词组成推荐使用"."分隔，这是习惯性做法，在Redis中键名本身不能包含空格

如：user : 1 : my.friends

### 8
生成自增ID

在MySQL等关系数据库中，可以通过字段属性为AUTO_INCREMENT来实现每增加一条记录自动为其生成一个唯一的递增ID，在Redis中可以使用INCR命令来实现这个效果

### 9
因为每个字符串类型键只能存储一个字符串，如果要存储博客文章由多个元素构造的对象时（博客包含：标题、正文、作者、发布时间等）可以先使用序列化函数将它们转换未一个字符串在存储

其他字符串类型还可以存储二进制数据，还可以使用MessagePack进行序列化

>MessagePack和JSON一样可以将对象序列化成字符串，序列化后结果占用空间更小，序列化后的结果是二进制格式


### 10

INCRBY命令增加指定整数

?>INCRBY key increment

``` bash
127.0.0.1:6379[1]> set num 1
OK
127.0.0.1:6379[1]> incr num
(integer) 2
127.0.0.1:6379[1]> INCRBY num 10
(integer) 12
```

DECR递减整数

DECRBY命令减少指定整数
?>DECRBY key decrement

```bash
127.0.0.1:6379[1]> DECR num
(integer) 11
127.0.0.1:6379[1]> DECRBY num 10
(integer) 1
```

增加指定浮点数

?>INCRBYFLOAT key increment

```bash
127.0.0.1:6379[1]> get num
"1"
127.0.0.1:6379[1]> INCRBYFLOAT num 6.66
"7.66"
```

向尾部追加值

?>APPEND key val

```bash
127.0.0.1:6379[1]> set s hello
OK
127.0.0.1:6379[1]> append s " ayuliao!"
(integer) 14
127.0.0.1:6379[1]> get s
"hello ayuliao!"
```

获取字符串长度

STRLEN key

```bash
127.0.0.1:6379[1]> get s
"hello ayuliao!"
127.0.0.1:6379[1]> STRLEN s
(integer) 14
```

同时获得/设置多个键值

```bash
127.0.0.1:6379[1]> mset key1 v1 key2 v2 key3 v3
OK
127.0.0.1:6379[1]> get key3
"v3"
127.0.0.1:6379[1]> mget key1 key2 key3
1) "v1"
2) "v2"
3) "v3"
```

### 11
Redis采用字典结构以键值对的形式存储数据，散列类型（hash）的键值也是一种字典结构，其存储了字段（field）和字段值的映射，但字段值只能是**字符串**，不支持其他数据类型

散列类型不能嵌套其他的数据类型，处理散列类型外，Redis其他数据类型同样不支持数据类型嵌套，比如集合的每个元素都只能是字符串，不能是另一个集合或散列列表

散列类型适合存储对象：使用对象类别和ID构成键名，使用字段表示对象的属性，而字段则存储属性值

### 12
如果使用ORM将关系数据库中的对象实体映射成程序中的实体，修改表结构往往意味中断服务，为了防止这种问题，在关系中数据库中存储这种半结构化数据还需要额外的表

而Redis的散列类型不存这样的问题，Redis存储结构完全是人为设定的，Redis并不要求每个键都依据此结构存储，完全可以自由的为任何键增减字段而不影响其他键

### 13

赋值:

?>HSET key field value
?>HMSET key field value [field value...]

取值:

?>HGET key field
?>HMGET key field [field...]

取全部值
HGETALL key



```bash
127.0.0.1:6379[1]> HSET human name ayuliao
(integer) 1
127.0.0.1:6379[1]> HSET human age 23
(integer) 1
127.0.0.1:6379[1]> HMSET human high 177 width 66
OK
127.0.0.1:6379[1]> HGET human name
"ayuliao"
127.0.0.1:6379[1]> HMGET human age high
1) "23"
2) "177
127.0.0.1:6379[1]> HGETALL human
1) "name"
2) "ayuliao"
3) "age"
4) "23"
5) "high"
6) "177"
7) "width"
8) "66"
```

### 14
HSET命令方便之处在于不区分插入和更新操作，在修改数据时不用事先判断字段是否存在来决定要执行的事插入操作还是更新操作

HSET执行插入操作时返回 1
HSET执行更新操作时返回 0

如果键不存在，HSET会自动创建它

### 15
Redis中每个键都输入一个明确的数据类型，如通过HSET命名建立的键是散列类型，通过SET命令建立的键是字符串类型

使用一种数据类型命名操作另一种数据类型的键会报错

### 16

HEXISTS命令判断一个字段是否存在，存在返回 1，不存在返回 0

```bash
127.0.0.1:6379[1]> HEXISTS human name
(integer) 1
127.0.0.1:6379[1]> HEXISTS human asdf
(integer) 0
```

使用HEXISTS命令当字段不存在时赋值

```
127.0.0.1:6379[1]> HEXISTS human asdf
(integer) 0
127.0.0.1:6379[1]> HSETNX human asdf ayu
(integer) 1
127.0.0.1:6379[1]> HGET human asdf
"ayu"
```

HINCRBY命令增加数字，返回值是增值后的字段值

```bash
127.0.0.1:6379[1]> HINCRBY human age 10
(integer) 33
```

HDEL命令用于删除字段，可删除多个字段

```bash
127.0.0.1:6379[1]> HDEL human name
(integer) 1
127.0.0.1:6379[1]> hget human name
(nil)
```

只获取字段名，HKEYS命令

```bash
127.0.0.1:6379[1]> HKEYS human
1) "age"
2) "high"
3) "width"
4) "asdf"
```

只获取字段值，HVALS命令

```bash
127.0.0.1:6379[1]> HVALS human
1) "33"
2) "177"
3) "66"
4) "ayu"
```

获得字段数，HLEN命令

```bash
127.0.0.1:6379[1]> HLEN human
(integer) 4
```

### 17

列表类型（list）可以存储一个**有序**字符串列表，常见操作时向列表两端添加元素或者获得列表某一个片段

列表类型内部使用双向链表实现，所以具有双向链表的优缺点。

列表类型的优点：可以快速的向列表两端添加、删除元素，时间复杂度都为O(1)，就算列表有1000万个元素，向尾部插入10个新元素也是特别快的

列表类型的缺点：通过索引访问元素比较慢，当列表中的数据特别多是，列表中间部分的数据访问比较慢

列表适合于社交网站的新鲜事，我们一般只关心最新的内容，使用列表存储这个数据，就算列表中有1000多万条，也只需取出头部100条，列表完整这个动作非常快速

### 18

从左端向列表增加元素，返回值为增加元素后列表的长度

```bash
127.0.0.1:6379[1]> LPUSH mylist 1
(integer) 1
127.0.0.1:6379[1]> LPUSH mylist 2
(integer) 2
127.0.0.1:6379[1]> LRANGE mylist 0 2
1) "2"
2) "1"
```

从右端向列表增加元素，返回值为增加元素后列表的长度

```bash
127.0.0.1:6379[1]> RPUSH mylist 0 -1
(integer) 4
127.0.0.1:6379[1]> LRANGE mylist 0 3
1) "2"
2) "1"
3) "0"
4) "-1"
```

从列表两端弹出元素

?>LPOP key
?>RPOP key

LPOP命令执行两个步骤：
1.将列表左边的元素从列表中移除
2.返回被移除的元素值

RPOP命令类似，移除列表右边的元素而已

```bash
127.0.0.1:6379[1]> LPOP mylist
"2"
127.0.0.1:6379[1]> RPOP mylist
"-1"
```

通过上面这4个命令，可以用列表来模拟栈和队列的操作

把列表当成栈，则搭配使用LPUSH和LPOP或RPUSH和RPOP
把列表当成队列，则搭配使用LPUSH和RPOP或RPUSH和LPOP

### 19

LRANGE命令获取列表中的片段

?>LRANGE key start stop

LRANGE命令将返回索引从start到stop之间的元素，与常见编程语言中返回列表时不同，LRANGE命令会返回**包含两端的元素**

LRANGE命令在获取数据的时不会删除该片段

LRANGE命令支持负索引，表示从右边开始计算序数

获取列表中所有元素

```bash
127.0.0.1:6379[1]> LRANGE mylist 0 -1
```

### 20

删除列表中指定的值

?>LREM key count value

LREM命令删除列表中前count个值为value的元素，返回值是实际删除的元素个数

count值不同，LREM执行方式不同

1.count>0, LREM会从列表左边开始删除前count个值为value的元素
2.count<0, LREM会从列表右边开始删除前 |count| 个值为value的元素
3.count=0, LREM会删除所有值为value的元素

```bash
127.0.0.1:6379[1]> LRANGE mylist 0 -1
1) "1"
2) "5"
3) "4"
4) "3"
5) "2"
6) "1"
7) "0"
# 删除从右边开始前2个值为“1”的元素
127.0.0.1:6379[1]> LREM mylist -2 1
(integer) 2
127.0.0.1:6379[1]> LRANGE mylist 0 -1
1) "5"
2) "4"
3) "3"
4) "2"
5) "0"
```

### 21
列表类型中没有类似字符串类型中的MGET命名，可以通过一个条命令同时获得多个键的键值的版本，如果单纯的通过命令一个个去获得多个键值，就需要时常请求数据库，每一次请求都会产生**往返时延**

?>往返时延：客户端请求命令发送到数据库到数据库返回响应数据给客户端所需的时间

### 22

获得/设置指定索引的元素值

?>LINDEX key index
?>LSET key index value

将列表类型当作数组来用，LINDEX命令不可少，LINDEX命令作用就是返回指定索引的元素，索引从0开始，支持负数索引，从右边开始搜索

```bash
127.0.0.1:6379[1]> LINDEX mylist 0
"5"
```

LSET通过索引来操作列表，会将索引为index的元素赋值为value

```bash
127.0.0.1:6379[1]> LSET mylist 0 7
OK
127.0.0.1:6379[1]> LINDEX mylist 0
"7"
```

只保留列表指定的片段

?> LTRIM key start end

LTRIM命令可以删除指定索引**范围之外**的所有元素

```bash
127.0.0.1:6379[1]> LRANGE mylist 0 -1
1) "7"
2) "4"
3) "3"
4) "2"
5) "0"
127.0.0.1:6379[1]> LTRIM mylist 1 2
OK
127.0.0.1:6379[1]> LRANGE mylist 0 -1
1) "4"
2) "3"
```

LTRIM命令场合LPUSH命令一起使用来限制列表中元素的数量，比如记录日志时值保留最新的100条日志，那么在每次加入新元素时调用一次LTRIM命令则可

```bash
LPUSH logs newlog
LTRIM logs 0 99
```

向列表中插入元素

?> LINSERT key BEFORE|AFTER pivot value

LINSERT命令首先会在列表中从左到右查找值为pivot的元素，然后根据第二个参数是BEFORE还说AFTER来觉得将value插入到该元素的前面还说后面

LINSERT命令返回值是插入后列表的元素个数

```bash
127.0.0.1:6379[1]> LRANGE mylist 0 -1
1) "4"
2) "3"
127.0.0.1:6379[1]> LINSERT mylist AFTER 3 6
(integer) 3
127.0.0.1:6379[1]> LRANGE mylist 0 -1
1) "4"
2) "3"
3) "6"
```

将一个元素从一个列表转到另一个列表

?> RPOPLPUSH source destination

它先执行RPOP命令再执行LPUSH命令，RPOPLPUSH会先从source列表类型键的右边弹出一个元素，然后将其加入到destination列表类型键的左边，并返回这个元素的值，整个过程是原子的

当把列表当做队列使用时，RPOPLPUSH命令可以很直观的在多个队列中传递参数

当source和destination是同一个队列时，RPOPLPUSH命令会不断将队尾元素移到对首，借助这个特性我们可以实现一个网站监控系统：使用一个队列存储需要监控的网址，监控程序不断的使用RPOPLPUSH命令循环取出一个网站来测试其可用性，这里使用RPOPLPUSH命令的好处是在执行过程中任然可以不断的向网址列表中加入新网址，整个系统容易扩展，允许多个客户端同时处理队列

###23

多个集合类型之间可以进行并集、交集和差集运算，非常便利

增加/删除元素

?> SADD key member [member...]
?> SREM key member [member...]

SADD向集合中添加元素，如果键不存在就会自动创建，因为一个集合中不能有相同的元素，所有加入相同元素是无效的

```bash
127.0.0.1:6379[1]> sadd myset 1 2 3
(integer) 3
127.0.0.1:6379[1]> sadd myset 1 2 3
(integer) 0
```

获得集合中所有元素

?> SMEMBERS key

```bash
127.0.0.1:6379[1]> SMEMBERS myset
1) "1"
2) "2"
3) "3"
```

判断元素是否存在集合中

?> SISMEMBER key member

```bash
127.0.0.1:6379[1]> SISMEMBER myset 1
(integer) 1
127.0.0.1:6379[1]> SISMEMBER myset 666
(integer) 0
```

集合间运算

?> SDIFF key [key...]
?> SINTER key [key...]
?> SUNION key [key...]

SDIFF 命令让多个集合执行差集运算

```bash
127.0.0.1:6379[1]> SADD setA 1 2 3
(integer) 3
127.0.0.1:6379[1]> SADD setB 2 3 4
(integer) 3
127.0.0.1:6379[1]> SDIFF setA setB
1) "1"
127.0.0.1:6379[1]> SDIFF setB setA
1) "4"
```

![](http://p6un02lk4.bkt.clouddn.com/seta-satb.png)

SDIFF 命令支持同时传入多个键

```bash
127.0.0.1:6379[1]> SADD setC 2 3
(integer) 2
127.0.0.1:6379[1]> SDIFF setA setB setC
1) "1"
```

计算顺序：先计算setA-setB，再计算结果与setC的差集

SINTER命令用于计算多个集合的交集运算

![](http://p6un02lk4.bkt.clouddn.com/setaandsatb.png)

```bash
127.0.0.1:6379[1]> SINTER setA setB
1) "2"
2) "3"
```

SINTER命令也可以支持同时传入多个键

SUNION命令对多个集合执行并集运算

![](http://p6un02lk4.bkt.clouddn.com/seta+setb.png)

```bash
127.0.0.1:6379[1]> SUNION setA setB
1) "1"
2) "2"
3) "3"
4) "4"
```

### 24

获得集合中元素的个数

?> SCARD key

SCARD命令用来获得集合中元素个数

```bash
127.0.0.1:6379[1]> SCARD setA
(integer) 3
```

进行集合运算并将结合存储

?>SDIFFSTORE destination key [key...]
?>SINTERSTORE destination key [key...]
?>SUNIONSTORE destination key [key...]

SDIFFSTORE命令与SDIFF命令功能一样，不同在于，SDIFFSTORE不会直接返回运算结果，而是将运算结果存到destination键中，另外两个命令也一样

随机获得集合中元素

?> SRANDMEMBER key [count]

SRANDMEMBER命令用来随机从集合中获取一个元素

```bash
127.0.0.1:6379[1]> SADD myset 1 2 3 4 5 6 7 8 9 10
(integer) 7
127.0.0.1:6379[1]> SRANDMEMBER myset
"10"
127.0.0.1:6379[1]> SRANDMEMBER myset
"10"
127.0.0.1:6379[1]> SRANDMEMBER myset
"4"
127.0.0.1:6379[1]> SRANDMEMBER myset
"3"
127.0.0.1:6379[1]> SRANDMEMBER myset
```

可以传入count参数来一次随机获得多个元素，更加count正负不同，表现不同

1.count>0,SRANDMEMBER会随机从集合中获得count个不重复的元素，如果count大于集合中元素的个数，那么SRANDMEMBER就会返回集合中全部元素

2.count<0,SRANDMEMBER会随机从集合里获得 |count| 个元素，这些元素可能是相同的

SRANDMEMBER命令返回的数据并没有想象中那么随机，这是因为**集合类型采用的存储结构（散列表）造成的**

散列表使用散列函数将元素映射到不同的存储位置上，以实现O(1)时间复杂度，如：

散列表存储元素b时，使用散列函数计算出b的散列值是0，所以将b存入编号为0桶，下次要查找b时通过同样的散列函数就再次计算出b存储的位置了

当两个不同的元素的散列值相同时会发生冲突，Redis使用拉链法来解决冲突问题，即将散列值冲突的元素以链表的形式存到同一个桶中

在使用SRANDMEMBER命令从集合中随机选择一个元素时，Redis首先会从所有的桶中随机选择一个桶，然后再从桶中的所有元素中随机选择一个元素

所以元素所在的桶中的元素数量越少，其被随机选中的可能性越大

![](http://p6un02lk4.bkt.clouddn.com/set%E6%95%A3%E5%88%97%E8%A1%A8.png)

从集合中弹出一个元素

?> SPOP key

因为集合中元素是随机存储的，所以SPOP命令会从集合中随机选择一个元素弹出

### 25

有序集合在集合类型的基础上，有序集合类型为集合中每个元素都关联的一个分数

通过获取集合中的分数，可以对集合中的元素进行排序、获取指定范围内分数的操作

### 26

增加元素

?> ZADD key score member [score member...]

ZADD命令用于向有序集合中加入一个元素和该元素的分数，如果该元素已经存在，则用新分数替换原有分数

ZADD命令返回值是新加入集合中的元素个数

```bash
127.0.0.1:6379[1]> ZADD sb 89 tom 67 peter 100 ayuliao
(integer) 3
127.0.0.1:6379[1]> zadd sb 150 ayuliao
(integer) 0
127.0.0.1:6379[1]> zadd sb 1.5 tom
(integer) 0
```

ZADD可以加入浮点数

+inf：无穷大
-inf：无穷小

获取排名在某个范围的元素列表

?> ZRANGE key start stop [WITHSCORES]
?> ZREVRANGE key start stop [WITHSCORES]


ZRANGE命令会按照元素分数从小到大的顺序返回索引从start到stop之间所有元素

ZRANGE命令与LRANGE命令非常相似，索引都是从0开始，负数代表从后向前查找

```bash
127.0.0.1:6379[1]> ZRANGE sb 0 2
1) "tom"
2) "peter"
3) "ayuliao"
```

获得所有元素

```bash
127.0.0.1:6379[1]> ZRANGE sb 0 -1
1) "tom"
2) "peter"
3) "ayuliao"
```

如果ZRANGE命令加上WITHSCORES参数，返回的数据格式就会从 "元素1，元素2...，元素n" ，变为 "元素1，分数1，元素2，分数2...，元素n，分数n

如果两个元素的分数相同，Redis就会按照 0<9<A<Z<a<z 顺序来排列

获取指定分数范围的元素

?> ZRANGEBYSCORE key min max [WITHSCORED] [LIMIT offset count]

ZRANGEBYSCORE命令会将元素从小到大的顺序返回分数在min和max之间的元素

```bash
127.0.0.1:6379[1]> ZRANGEBYSCORE sb 70 150
1) "ayuliao"
```

如果希望分数范围不包含端点值，可以在分数前加上 "("符合

```bash
127.0.0.1:6379[1]> ZRANGEBYSCORE sb 70 (150
(empty list or set)
```

min和max还支持无穷小和无穷大

```bash
127.0.0.1:6379[1]> ZRANGEBYSCORE sb -inf +inf
1) "tom"
2) "peter"
3) "ayuliao"
```

WITHSCORES参数用法与ZRANGE命令一样

```bash
127.0.0.1:6379[1]> ZRANGEBYSCORE sb 80 100 WITHSCORES
1) "tom"
2) "89"
3) "ayuliao"
4) "100"
```

Redis中的LIMIT跟MySQL中的LIMIT效果用法都类似

```bash
127.0.0.1:6379[1]> ZRANGEBYSCORE sb 80 +inf LIMIT 1 3
1) "ayuliao"
```

ZREVRANGEBYSCORE命令不仅是按照元素分数从大王小的顺序给出结果的，而且它的min和max参数的顺序和ZRANGERBYSCORE命令相反

增加某个元素的分数

?>ZINCRBY key increment member

ZINCRBY命令可以增加一个元素的分数，返回值是更改后的分数

```bash
127.0.0.1:6379[1]> ZINCRBY sb 100 ayuliao
"200"
127.0.0.1:6379[1]> ZINCRBY sb -100 ayuliao
"100"
```

如果ZINCRBY命令指定的元素不存在，Redis在执行命令前后先建立它并将它分数赋值为0在执行操作

### 27

获得集合中元素的数量

?> ZCARD key

```bash
127.0.0.1:6379[1]> ZCARD sb
(integer) 3
```

获得指定分数范围内的元素个数

?>ZCOUNT key min max

```bash
127.0.0.1:6379[1]> ZCOUNT sb 80 100
(integer) 2
```

删除一个或多个元素

?>ZREM key member

```bash
127.0.0.1:6379[1]> ZREM sb tom peter
(integer) 2
127.0.0.1:6379[1]> ZCARD sb
(integer) 1
```

按照排名范围删除元素

?>ZREMRANGEBYRANK key start stop

```bash
127.0.0.1:6379[1]> ZADD sb 80 test1 30 test2 40 test3 50 test4
(integer) 4
127.0.0.1:6379[1]> ZREMRANGEBYRANK sb 0 2
(integer) 3
127.0.0.1:6379[1]> ZRANGE sb 0 -1
1) "test1"
2) "ayuliao"
127.0.0.1:6
```

按分数范围删除元素

?>ZREMRANGEBYSCORE key min max

```bash
127.0.0.1:6379[1]> ZADD sb 80 test1 30 test2 40 test3 50 test4
(integer) 3
127.0.0.1:6379[1]> ZREMRANGEBYSCORE sb (30 80
(integer) 3
127.0.0.1:6379[1]> ZRANGE sb 0 -1
1) "test2"
2) "ayuliao"
```

获得元素的排名

?>ZRANK key member

0开始，从小到大排序

```bash
127.0.0.1:6379[1]> ZRANK sb ayuliao
(integer) 1
```

?>ZREVRANK key member

0开始，从大到小排序，分数最大的元素排名为0

```bash
127.0.0.1:6379[1]> ZREVRANK sb ayuliao
(integer) 0
```

计算有序集合的交集


