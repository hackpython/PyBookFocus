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









