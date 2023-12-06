---
title: Redis学习笔记语法
tags: [Redis, 微服务]
categories: 学习笔记
date: 2023-11-15 19:20:00
---

# Redis学习笔记

## 安装

#### docker-compose.yml

```dockerfile
version: '3.3'
services:
    redis: 
        image: redis:6.2.14
        hostname: redis
        container_name: redis
        privileged: true
        ports:
          - 6379:6379
        environment:
          TZ: Asia/Shanghai
        volumes:
          - ./data:/data
          - ./conf/redis.conf:/usr/local/redis/conf/redis.conf
          - ./logs:/logs
        command: ["redis-server","/usr/local/redis/conf/redis.conf"]

```

#### 配置文件(./conf/redis.conf)

```shell
# Redis配置文件样例
 
# Note on units: when memory size is needed, it is possible to specifiy
# it in the usual form of 1k 5GB 4M and so forth:
#
# 1k => 1000 bytes
# 1kb => 1024 bytes
# 1m => 1000000 bytes
# 1mb => 1024*1024 bytes
# 1g => 1000000000 bytes
# 1gb => 1024*1024*1024 bytes
#
# units are case insensitive so 1GB 1Gb 1gB are all the same.
 
# Redis默认不是以守护进程的方式运行，可以通过该配置项修改，使用yes启用守护进程
# 启用守护进程后，Redis会把pid写到一个pidfile中，在/var/run/redis.pid
daemonize no
 
# 当Redis以守护进程方式运行时，Redis默认会把pid写入/var/run/redis.pid文件，可以通过pidfile指定
pidfile /var/run/redis.pid
 
# 指定Redis监听端口，默认端口为6379
# 如果指定0端口，表示Redis不监听TCP连接
port 6379
 
# 绑定的主机地址
# 你可以绑定单一接口，如果没有绑定，所有接口都会监听到来的连接
# bind 127.0.0.1
# 2、关闭保护机制
protected-mode no
 
# Specify the path for the unix socket that will be used to listen for
# incoming connections. There is no default, so Redis will not listen
# on a unix socket when not specified.
#
# unixsocket /tmp/redis.sock
# unixsocketperm 755
 
# 当客户端闲置多长时间后关闭连接，如果指定为0，表示关闭该功能
timeout 0
 
# 指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为verbose
# debug (很多信息, 对开发／测试比较有用)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel verbose
 
# 日志记录方式，默认为标准输出，如果配置为redis为守护进程方式运行，而这里又配置为标准输出，则日志将会发送给/dev/null
logfile /logs/redis.log
 
# To enable logging to the system logger, just set 'syslog-enabled' to yes,
# and optionally update the other syslog parameters to suit your needs.
# syslog-enabled no
 
# Specify the syslog identity.
# syslog-ident redis
 
# Specify the syslog facility.  Must be USER or between LOCAL0-LOCAL7.
# syslog-facility local0
 
# 设置数据库的数量，默认数据库为0，可以使用select <dbid>命令在连接上指定数据库id
# dbid是从0到‘databases’-1的数目
databases 16
 
################################ SNAPSHOTTING  #################################
# 指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合
# Save the DB on disk:
#
#   save <seconds> <changes>
#
#   Will save the DB if both the given number of seconds and the given
#   number of write operations against the DB occurred.
#
#   满足以下条件将会同步数据:
#   900秒（15分钟）内有1个更改
#   300秒（5分钟）内有10个更改
#   60秒内有10000个更改
#   Note: 可以把所有“save”行注释掉，这样就取消同步操作了
 
save 900 1
save 300 10
save 60 10000
 
# 指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，可以关闭该选项，但会导致数据库文件变的巨大
rdbcompression yes
 
# 指定本地数据库文件名，默认值为dump.rdb
dbfilename dump.rdb
 
# 工作目录.
# 指定本地数据库存放目录，文件名由上一个dbfilename配置项指定
#
# Also the Append Only File will be created inside this directory.
#
# 注意，这里只能指定一个目录，不能指定文件名
dir ./
 
################################# REPLICATION #################################
 
# 主从复制。使用slaveof从 Redis服务器复制一个Redis实例。注意，该配置仅限于当前slave有效
# so for example it is possible to configure the slave to save the DB with a
# different interval, or to listen to another port, and so on.
# 设置当本机为slav服务时，设置master服务的ip地址及端口，在Redis启动时，它会自动从master进行数据同步
# slaveof <masterip> <masterport>
 
 
# 当master服务设置了密码保护时，slav服务连接master的密码
# 下文的“requirepass”配置项可以指定密码
# masterauth <master-password>
 
# When a slave lost the connection with the master, or when the replication
# is still in progress, the slave can act in two different ways:
#
# 1) if slave-serve-stale-data is set to 'yes' (the default) the slave will
#    still reply to client requests, possibly with out of data data, or the
#    data set may just be empty if this is the first synchronization.
#
# 2) if slave-serve-stale data is set to 'no' the slave will reply with
#    an error "SYNC with master in progress" to all the kind of commands
#    but to INFO and SLAVEOF.
#
slave-serve-stale-data yes
 
# Slaves send PINGs to server in a predefined interval. It's possible to change
# this interval with the repl_ping_slave_period option. The default value is 10
# seconds.
#
# repl-ping-slave-period 10
 
# The following option sets a timeout for both Bulk transfer I/O timeout and
# master data or ping response timeout. The default value is 60 seconds.
#
# It is important to make sure that this value is greater than the value
# specified for repl-ping-slave-period otherwise a timeout will be detected
# every time there is low traffic between the master and the slave.
#
# repl-timeout 60
 
################################## SECURITY ###################################
 
# Warning: since Redis is pretty fast an outside user can try up to
# 150k passwords per second against a good box. This means that you should
# use a very strong password otherwise it will be very easy to break.
# 设置Redis连接密码，如果配置了连接密码，客户端在连接Redis时需要通过auth <password>命令提供密码，默认关闭
requirepass 123456
# Command renaming.
#
# It is possilbe to change the name of dangerous commands in a shared
# environment. For instance the CONFIG command may be renamed into something
# of hard to guess so that it will be still available for internal-use
# tools but not available for general clients.
#
# Example:
#
# rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52
#
# It is also possilbe to completely kill a command renaming it into
# an empty string:
#
# rename-command CONFIG ""
 
################################### LIMITS ####################################
 
# 设置同一时间最大客户端连接数，默认无限制，Redis可以同时打开的客户端连接数为Redis进程可以打开的最大文件描述符数，
# 如果设置maxclients 0，表示不作限制。当客户端连接数到达限制时，Redis会关闭新的连接并向客户端返回max Number of clients reached错误信息
# maxclients 128
 
# Don't use more memory than the specified amount of bytes.
# When the memory limit is reached Redis will try to remove keys with an
# EXPIRE set. It will try to start freeing keys that are going to expire
# in little time and preserve keys with a longer time to live.
# Redis will also try to remove objects from free lists if possible.
#
# If all this fails, Redis will start to reply with errors to commands
# that will use more memory, like SET, LPUSH, and so on, and will continue
# to reply to most read-only commands like GET.
#
# WARNING: maxmemory can be a good idea mainly if you want to use Redis as a
# 'state' server or cache, not as a real DB. When Redis is used as a real
# database the memory usage will grow over the weeks, it will be obvious if
# it is going to use too much memory in the long run, and you'll have the time
# to upgrade. With maxmemory after the limit is reached you'll start to get
# errors for write operations, and this may even lead to DB inconsistency.
# 指定Redis最大内存限制，Redis在启动时会把数据加载到内存中，达到最大内存后，Redis会先尝试清除已到期或即将到期的Key，
# 当此方法处理后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。
# Redis新的vm机制，会把Key存放内存，Value会存放在swap区
# maxmemory <bytes>
 
# MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
# is reached? You can select among five behavior:
#
# volatile-lru -> remove the key with an expire set using an LRU algorithm
# allkeys-lru -> remove any key accordingly to the LRU algorithm
# volatile-random -> remove a random key with an expire set
# allkeys->random -> remove a random key, any key
# volatile-ttl -> remove the key with the nearest expire time (minor TTL)
# noeviction -> don't expire at all, just return an error on write operations
#
# Note: with all the kind of policies, Redis will return an error on write
#       operations, when there are not suitable keys for eviction.
#
#       At the date of writing this commands are: set setnx setex append
#       incr decr rpush lpush rpushx lpushx linsert lset rpoplpush sadd
#       sinter sinterstore sunion sunionstore sdiff sdiffstore zadd zincrby
#       zunionstore zinterstore hset hsetnx hmset hincrby incrby decrby
#       getset mset msetnx exec sort
#
# The default is:
#
# maxmemory-policy volatile-lru
 
# LRU and minimal TTL algorithms are not precise algorithms but approximated
# algorithms (in order to save memory), so you can select as well the sample
# size to check. For instance for default Redis will check three keys and
# pick the one that was used less recently, you can change the sample size
# using the following configuration directive.
#
# maxmemory-samples 3
 
############################## APPEND ONLY MODE ###############################
 
#
# Note that you can have both the async dumps and the append only file if you
# like (you have to comment the "save" statements above to disable the dumps).
# Still if append only mode is enabled Redis will load the data from the
# log file at startup ignoring the dump.rdb file.
# 指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。
# 因为redis本身同步数据文件是按上面save条件来同步的，所以有的数据会在一段时间内只存在于内存中。默认为no
# IMPORTANT: Check the BGREWRITEAOF to check how to rewrite the append
# log file in background when it gets too big.
 
appendonly yes
 
# 指定更新日志文件名，默认为appendonly.aof
# appendfilename appendonly.aof
 
# The fsync() call tells the Operating System to actually write data on disk
# instead to wait for more data in the output buffer. Some OS will really flush
# data on disk, some other OS will just try to do it ASAP.
 
# 指定更新日志条件，共有3个可选值：
# no:表示等操作系统进行数据缓存同步到磁盘（快）
# always:表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全）
# everysec:表示每秒同步一次（折衷，默认值）
 
appendfsync everysec
# appendfsync no
 
# When the AOF fsync policy is set to always or everysec, and a background
# saving process (a background save or AOF log background rewriting) is
# performing a lot of I/O against the disk, in some Linux configurations
# Redis may block too long on the fsync() call. Note that there is no fix for
# this currently, as even performing fsync in a different thread will block
# our synchronous write(2) call.
#
# In order to mitigate this problem it's possible to use the following option
# that will prevent fsync() from being called in the main process while a
# BGSAVE or BGREWRITEAOF is in progress.
#
# This means that while another child is saving the durability of Redis is
# the same as "appendfsync none", that in pratical terms means that it is
# possible to lost up to 30 seconds of log in the worst scenario (with the
# default Linux settings).
#
# If you have latency problems turn this to "yes". Otherwise leave it as
# "no" that is the safest pick from the point of view of durability.
no-appendfsync-on-rewrite no
 
# Automatic rewrite of the append only file.
# Redis is able to automatically rewrite the log file implicitly calling
# BGREWRITEAOF when the AOF log size will growth by the specified percentage.
#
# This is how it works: Redis remembers the size of the AOF file after the
# latest rewrite (or if no rewrite happened since the restart, the size of
# the AOF at startup is used).
#
# This base size is compared to the current size. If the current size is
# bigger than the specified percentage, the rewrite is triggered. Also
# you need to specify a minimal size for the AOF file to be rewritten, this
# is useful to avoid rewriting the AOF file even if the percentage increase
# is reached but it is still pretty small.
#
# Specify a precentage of zero in order to disable the automatic AOF
# rewrite feature.
 
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
 
################################## SLOW LOG ###################################
 
# The Redis Slow Log is a system to log queries that exceeded a specified
# execution time. The execution time does not include the I/O operations
# like talking with the client, sending the reply and so forth,
# but just the time needed to actually execute the command (this is the only
# stage of command execution where the thread is blocked and can not serve
# other requests in the meantime).
#
# You can configure the slow log with two parameters: one tells Redis
# what is the execution time, in microseconds, to exceed in order for the
# command to get logged, and the other parameter is the length of the
# slow log. When a new command is logged the oldest one is removed from the
# queue of logged commands.
 
# The following time is expressed in microseconds, so 1000000 is equivalent
# to one second. Note that a negative number disables the slow log, while
# a value of zero forces the logging of every command.
slowlog-log-slower-than 10000
 
# There is no limit to this length. Just be aware that it will consume memory.
# You can reclaim memory used by the slow log with SLOWLOG RESET.
slowlog-max-len 1024
 
################################ VIRTUAL MEMORY ###############################
 
### WARNING! Virtual Memory is deprecated in Redis 2.4
### The use of Virtual Memory is strongly discouraged.
 
### WARNING! Virtual Memory is deprecated in Redis 2.4
### The use of Virtual Memory is strongly discouraged.
 
# Virtual Memory allows Redis to work with datasets bigger than the actual
# amount of RAM needed to hold the whole dataset in memory.
# In order to do so very used keys are taken in memory while the other keys
# are swapped into a swap file, similarly to what operating systems do
# with memory pages.
# 指定是否启用虚拟内存机制，默认值为no，
# VM机制将数据分页存放，由Redis将访问量较少的页即冷数据swap到磁盘上，访问多的页面由磁盘自动换出到内存中
# 把vm-enabled设置为yes，根据需要设置好接下来的三个VM参数，就可以启动VM了
# vm-enabled no
# vm-enabled yes
 
# This is the path of the Redis swap file. As you can guess, swap files
# can't be shared by different Redis instances, so make sure to use a swap
# file for every redis process you are running. Redis will complain if the
# swap file is already in use.
#
# Redis交换文件最好的存储是SSD（固态硬盘）
# 虚拟内存文件路径，默认值为/tmp/redis.swap，不可多个Redis实例共享
# *** WARNING *** if you are using a shared hosting the default of putting
# the swap file under /tmp is not secure. Create a dir with access granted
# only to Redis user and configure Redis to create the swap file there.
# vm-swap-file /tmp/redis.swap
 
# With vm-max-memory 0 the system will swap everything it can. Not a good
# default, just specify the max amount of RAM you can in bytes, but it's
# better to leave some margin. For instance specify an amount of RAM
# that's more or less between 60 and 80% of your free RAM.
# 将所有大于vm-max-memory的数据存入虚拟内存，无论vm-max-memory设置多少，所有索引数据都是内存存储的（Redis的索引数据就是keys）
# 也就是说当vm-max-memory设置为0的时候，其实是所有value都存在于磁盘。默认值为0
# vm-max-memory 0
 
# Redis swap文件分成了很多的page，一个对象可以保存在多个page上面，但一个page上不能被多个对象共享，vm-page-size是要根据存储的数据大小来设定的。
# 建议如果存储很多小对象，page大小最后设置为32或64bytes；如果存储很大的对象，则可以使用更大的page，如果不确定，就使用默认值
# vm-page-size 32
 
# 设置swap文件中的page数量由于页表（一种表示页面空闲或使用的bitmap）是存放在内存中的，在磁盘上每8个pages将消耗1byte的内存
# swap空间总容量为 vm-page-size * vm-pages
#
# With the default of 32-bytes memory pages and 134217728 pages Redis will
# use a 4 GB swap file, that will use 16 MB of RAM for the page table.
#
# It's better to use the smallest acceptable value for your application,
# but the default is large in order to work in most conditions.
# vm-pages 134217728
 
# Max number of VM I/O threads running at the same time.
# This threads are used to read/write data from/to swap file, since they
# also encode and decode objects from disk to memory or the reverse, a bigger
# number of threads can help with big objects even if they can't help with
# I/O itself as the physical device may not be able to couple with many
# reads/writes operations at the same time.
# 设置访问swap文件的I/O线程数，最后不要超过机器的核数，如果设置为0，那么所有对swap文件的操作都是串行的，可能会造成比较长时间的延迟，默认值为4
# vm-max-threads 4
 
############################### ADVANCED CONFIG ###############################
 
# Hashes are encoded in a special way (much more memory efficient) when they
# have at max a given numer of elements, and the biggest element does not
# exceed a given threshold. You can configure this limits with the following
# configuration directives.
# 指定在超过一定的数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法
# hash-max-zipmap-entries 512
# hash-max-zipmap-value 64
 
# Similarly to hashes, small lists are also encoded in a special way in order
# to save a lot of space. The special representation is only used when
# you are under the following limits:
list-max-ziplist-entries 512
list-max-ziplist-value 64
 
# Sets have a special encoding in just one case: when a set is composed
# of just strings that happens to be integers in radix 10 in the range
# of 64 bit signed integers.
# The following configuration setting sets the limit in the size of the
# set in order to use this special memory saving encoding.
set-max-intset-entries 512
 
# Similarly to hashes and lists, sorted sets are also specially encoded in
# order to save a lot of space. This encoding is only used when the length and
# elements of a sorted set are below the following limits:
zset-max-ziplist-entries 128
zset-max-ziplist-value 64
 
# Active rehashing uses 1 millisecond every 100 milliseconds of CPU time in
# order to help rehashing the main Redis hash table (the one mapping top-level
# keys to values). The hash table implementation redis uses (see dict.c)
# performs a lazy rehashing: the more operation you run into an hash table
# that is rhashing, the more rehashing "steps" are performed, so if the
# server is idle the rehashing is never complete and some more memory is used
# by the hash table.
#
# The default is to use this millisecond 10 times every second in order to
# active rehashing the main dictionaries, freeing memory when possible.
#
# If unsure:
# use "activerehashing no" if you have hard latency requirements and it is
# not a good thing in your environment that Redis can reply form time to time
# to queries with 2 milliseconds delay.
# 指定是否激活重置哈希，默认为开启
activerehashing yes
 
################################## INCLUDES ###################################
 
# 指定包含其他的配置文件，可以在同一主机上多个Redis实例之间使用同一份配置文件，而同时各实例又拥有自己的特定配置文件
# include /path/to/local.conf
# include /path/to/other.conf
```

## 简介

Redis 是完全开源的，遵守 BSD 协议，是一个高性能的 key-value 数据库。

Redis 与其他 key - value 缓存产品有以下三个特点：

* Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
* Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
* Redis支持数据的备份，即master-slave模式的数据备份。

简单来说 redis 就是一个数据库，不过与传统数据库不同的是 redis 的数据是存在内存中的，所以读写速度非常快，因此 redis 被广泛应用于缓存方向。另外，redis 也经常用来做[分布式锁](https://so.csdn.net/so/search?q=分布式锁&spm=1001.2101.3001.7020)。

### Redis 优势

* 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
* 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
* 原子 – Redis的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。单个操作是原子性的。多个操作也支持事务，即原子性，通过MULTI和EXEC指令包起来。
* 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。

------

### Redis与其他key-value存储有什么不同？

* Redis有着更为复杂的数据结构并且提供对他们的原子性操作，这是一个不同于其他数据库的进化路径。Redis的数据类型都是基于基本数据结构的同时对程序员透明，无需进行额外的抽象。
* Redis运行在内存中但是可以持久化到磁盘，所以在对不同数据集进行高速读写时需要权衡内存，因为数据量不能大于硬件内存。在内存数据库方面的另一个优点是，相比在磁盘上相同的复杂的数据结构，在内存中操作起来非常简单，这样Redis可以做很多内部复杂性很强的事情。同时，在磁盘格式方面他们是紧凑的以追加的方式产生的，因为他们并不需要进行随机访问。

## 数据库操作

### 切换数据库

```
SELECT <db_number>
```

### 用于清空当前选择的数据库

`Flush Database`

```
FLUSHDB
```

> （默认是数据库编号为0的数据库）中的所有数据。

## 数据操作

### 数据类型

Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。

### 键(keys )命令

| 命令                 | 描述                        |
| -------------------- | --------------------------- |
| `SET key value`      | 设置键的值。                |
| `GET key`            | 获取键的值。                |
| `DEL key [key2 ...]` | 删除一个或多个键。          |
| `EXISTS key`         | 检查键是否存在。            |
| `KEYS pattern`       | 查找匹配指定模式的键。      |
| `TYPE key`           | 返回 key 所储存的值的类型。 |

### 字符串(String)

下表列出了常用的 redis 字符串命令：

| `命令`                           | `描述`                                                       |
| -------------------------------- | ------------------------------------------------------------ |
| `SET key value`                  | 设置指定 key 的值。                                          |
| `GET key`                        | 获取指定 key 的值。                                          |
| `GETRANGE key start end`         | 返回 key 中字符串值的子字符。`start` 和 `end`: 子字符串的起始和结束偏移量。 |
| `GETSET key value`               | 将给定 key 的值设为 value，并返回 key 的旧值(old value)。    |
| `SETEX key seconds value`        | 将值 value 关联到 key，并将 key 的过期时间设为 seconds (以秒为单位)。 |
| `SETNX key value`                | 只有在 key 不存在时设置 key 的值。                           |
| `STRLEN key`                     | 返回 key 所储存的字符串值的长度。                            |
| `MSET key value key value ...`   | 同时设置一个或多个 key-value 对。                            |
| `MSETNX key value key value ...` | 同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在。 |
| `PSETEX key milliseconds value`  | 以毫秒为单位设置 key 的生存时间，与SETEX命令相似，但以毫秒为单位。 |
| `INCR key`                       | 将 key 中储存的数字值增一。                                  |
| `INCRBY key increment`           | 将 key 所储存的值加上给定的增量值（increment）。             |
| `INCRBYFLOAT key increment`      | 将 key 所储存的值加上给定的浮点增量值（increment）。         |
| `DECR key`                       | 将 key 中储存的数字值减一。                                  |
| `DECRBY key decrement`           | 将 key 所储存的值减去给定的减量值（decrement）。             |

### 哈希(Hash)

Redis hash 是一个 string 类型的 field（字段） 和 value（值） 的映射表，hash 特别适合用于存储对象。

Redis 中每个 hash 可以存储 232 - 1 键值对（40多亿）。

以下是 Redis 哈希（Hash）相关的基本命令及其描述：

| 命令                                             | 描述                                                    |
| ------------------------------------------------ | ------------------------------------------------------- |
| `HDEL key field1 [field2]`                       | 删除一个或多个哈希表字段。                              |
| `HEXISTS key field`                              | 查看哈希表 key 中，指定的字段是否存在。                 |
| `HGET key field`                                 | 获取存储在哈希表中指定字段的值。                        |
| `HGETALL key`                                    | 获取在哈希表中指定 key 的所有字段和值。                 |
| `HINCRBY key field increment`                    | 为哈希表 key 中的指定字段的整数值加上增量 increment。   |
| `HINCRBYFLOAT key field increment`               | 为哈希表 key 中的指定字段的浮点数值加上增量 increment。 |
| `HKEYS key`                                      | 获取哈希表中的所有字段。                                |
| `HLEN key`                                       | 获取哈希表中字段的数量。                                |
| `HMGET key field1 [field2]`                      | 获取所有给定字段的值。                                  |
| `HMSET key field1 value1 [field2 value2]`        | 同时将多个 field-value (域-值)对设置到哈希表 key 中。   |
| `HSET key field value`                           | 将哈希表 key 中的字段 field 的值设为 value。            |
| `HSETNX key field value`                         | 只有在字段 field 不存在时，设置哈希表字段的值。         |
| `HVALS key`                                      | 获取哈希表中所有值。                                    |
| `HSCAN key cursor [MATCH pattern] [COUNT count]` | 迭代哈希表中的键值对。                                  |

### 列表(List)

Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）

一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。

以下是列表相关的基本命令及其描述：

| 命令                                    | 描述                                                         |
| --------------------------------------- | ------------------------------------------------------------ |
| `BLPOP key1 [key2 ] timeout`            | 移出并获取列表的第一个元素，如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| `BRPOP key1 [key2 ] timeout`            | 移出并获取列表的最后一个元素，如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| `BRPOPLPUSH source destination timeout` | 从列表中弹出一个值，将弹出的元素插入到另外一个列表中并返回它；如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| `LINDEX key index`                      | 通过索引获取列表中的元素。                                   |
| `LINSERT key BEFORE`                    | AFTER pivot value`                                           |
| `LLEN key`                              | 获取列表长度。                                               |
| `LPOP key`                              | 移出并获取列表的第一个元素。                                 |
| `LPUSH key value1 [value2]`             | 将一个或多个值插入到列表头部。                               |
| `LPUSHX key value`                      | 将一个值插入到已存在的列表头部。                             |
| `LRANGE key start stop`                 | 获取列表指定范围内的元素。                                   |
| `LREM key count value`                  | 移除列表元素。                                               |
| `LSET key index value`                  | 通过索引设置列表元素的值。                                   |
| `LTRIM key start stop`                  | 对一个列表进行修剪(trim)，即只保留指定区间内的元素，不在指定区间之内的元素都将被删除。 |
| `RPOP key`                              | 移除列表的最后一个元素，返回值为移除的元素。                 |
| `RPOPLPUSH source destination`          | 移除列表的最后一个元素，并将该元素添加到另一个列表并返回。   |
| `RPUSH key value1 [value2]`             | 在列表中添加一个或多个值到列表尾部。                         |
| `RPUSHX key value`                      | 为已存在的列表添加值。                                       |

### 集合(Set)

Redis 的 Set 是 String 类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。

集合对象的编码可以是 intset 或者 hashtable。



Redis 中集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。

集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。

以下是 Redis 集合基本命令及其描述：

| 命令                                             | 描述                                                  |
| ------------------------------------------------ | ----------------------------------------------------- |
| `SADD key member1 [member2]`                     | 向集合添加一个或多个成员。                            |
| `SCARD key`                                      | 获取集合的成员数。                                    |
| `SDIFF key1 [key2]`                              | 返回第一个集合与其他集合之间的差异。                  |
| `SDIFFSTORE destination key1 [key2]`             | 返回给定所有集合的差集并存储在 destination 中。       |
| `SINTER key1 [key2]`                             | 返回给定所有集合的交集。                              |
| `SINTERSTORE destination key1 [key2]`            | 返回给定所有集合的交集并存储在 destination 中。       |
| `SISMEMBER key member`                           | 判断 member 元素是否是集合 key 的成员。               |
| `SMEMBERS key`                                   | 返回集合中的所有成员。                                |
| `SMOVE source destination member`                | 将 member 元素从 source 集合移动到 destination 集合。 |
| `SPOP key`                                       | 移除并返回集合中的一个随机元素。                      |
| `SRANDMEMBER key [count]`                        | 返回集合中一个或多个随机元素。                        |
| `SREM key member1 [member2]`                     | 移除集合中一个或多个成员。                            |
| `SUNION key1 [key2]`                             | 返回所有给定集合的并集。                              |
| `SUNIONSTORE destination key1 [key2]`            | 所有给定集合的并集存储在 destination 集合中。         |
| `SSCAN key cursor [MATCH pattern] [COUNT count]` | 迭代集合中的元素。                                    |

这些命令用于对 Redis 中的集合数据结构进行操作，允许你添加、删除、获取成员，以及执行集合之间的差集、交集、并集等操作。

### 有序集合(sortedset)

Redis 有序集合和集合一样也是 string 类型元素的集合,且不允许重复的成员。

不同的是每个元素都会关联一个 double 类型的分数。redis 正是通过分数来为集合中的成员进行从小到大的排序。

有序集合的成员是唯一的,但分数(score)却可以重复。

集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。 集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。

| 命令                                             | 描述                                                         |
| ------------------------------------------------ | ------------------------------------------------------------ |
| `ZADD key score1 member1 [score2 member2]`       | 向有序集合添加一个或多个成员，或者更新已存在成员的分数。     |
| `ZCARD key`                                      | 获取有序集合的成员数。                                       |
| `ZCOUNT key min max`                             | 计算在有序集合中指定区间分数的成员数。                       |
| `ZINCRBY key increment member`                   | 有序集合中对指定成员的分数加上增量 increment。               |
| `ZINTERSTORE destination numkeys key [key ...]`  | 计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 destination 中。 |
| `ZLEXCOUNT key min max`                          | 在有序集合中计算指定字典区间内成员数量。                     |
| `ZRANGE key start stop [WITHSCORES]`             | 通过索引区间返回有序集合指定区间内的成员。                   |
| `ZRANGEBYLEX key min max [LIMIT offset count]`   | 通过字典区间返回有序集合的成员。                             |
| `ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT]` | 通过分数返回有序集合指定区间内的成员。                       |
| `ZRANK key member`                               | 返回有序集合中指定成员的索引。                               |
| `ZREM key member [member ...]`                   | 移除有序集合中的一个或多个成员。                             |
| `ZREMRANGEBYLEX key min max`                     | 移除有序集合中给定的字典区间的所有成员。                     |
| `ZREMRANGEBYRANK key start stop`                 | 移除有序集合中给定的排名区间的所有成员。                     |
| `ZREMRANGEBYSCORE key min max`                   | 移除有序集合中给定的分数区间的所有成员。                     |
| `ZREVRANGE key start stop [WITHSCORES]`          | 返回有序集中指定区间内的成员，通过索引，分数从高到低。       |
| `ZREVRANGEBYSCORE key max min [WITHSCORES]`      | 返回有序集中指定分数区间内的成员，分数从高到低排序。         |
| `ZREVRANK key member`                            | 返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序。 |
| `ZSCORE key member`                              | 返回有序集中，成员的分数值。                                 |
| `ZUNIONSTORE destination numkeys key [key ...]`  | 计算给定的一个或多个有序集的并集，并存储在新的 key 中。      |
| `ZSCAN key cursor [MATCH pattern] [COUNT count]` | 迭代有序集合中的元素（包括元素成员和元素分值）。             |

## 进阶操作

### SQL脚本

下面代码为此次练习用的数据：

```
select 0
SET username "JohnDoe"
SET email "johndoe@example.com"
SET website "www.example.com"
SET city "New York"
SET country "USA"
SET company "ABC Corporation"
SET industry "Tech"
SET revenue 5000000

HMSET user:1 username "Alice" email "alice@example.com" age 25
HMSET user:2 username "Bob" email "bob@example.com" age 30
HMSET product:1 name "Laptop" price 1200 brand "Dell"
HMSET product:2 name "Smartphone" price 800 brand "Samsung"
HMSET product:3 name "Headphones" price 150 brand "Sony"
HMSET user:3 username "Eve" email "eve@example.com" age 28
HMSET user:4 username "David" email "david@example.com" age 35

RPUSH messages "Hello, Redis!"
RPUSH messages "Welcome to Redis!"
RPUSH tasks "Task 1"
RPUSH tasks "Task 2"
RPUSH tasks "Task 3"
RPUSH messages "How are you?"
RPUSH messages "Redis is awesome!"

SADD tags "redis" "database" "nosql"
SADD interests "Technology" "Travel" "Photography"
SADD colors "Red" "Green" "Blue"
 
ZADD leaderboard 100 "Alice"
ZADD leaderboard 150 "Bob"
ZADD leaderboard 120 "Charlie"
ZADD highscores 950 "PlayerA"
ZADD highscores 1100 "PlayerB"
ZADD highscores 850 "PlayerC"
ZADD highscores 1200 "PlayerD"
ZADD ratings 4.5 "ProductX"
ZADD ratings 3.8 "ProductY"
ZADD ratings 5.0 "ProductZ"
```



## 高级操作

### 数据库备份和恢复

#### 备份

Redis **SAVE** 命令用于创建当前数据库的备份。

redis Save 命令基本语法如下：

```
SAVE 
```

#### 恢复数据

如果需要恢复数据，只需将备份文件 (dump.rdb) 移动到 redis 安装目录并启动服务即可。获取 redis 目录可以使用 **CONFIG** 命令，如下所示：

```
redis 127.0.0.1:6379> CONFIG GET dir
1) "dir"
2) "/usr/local/redis/bin"
```

以上命令 **CONFIG GET dir** 输出的 redis 安装目录为 /usr/local/redis/bin。

### Bgsave

创建 redis 备份文件也可以使用命令 **BGSAVE**，该命令在后台执行。

**实例**

```
127.0.0.1:6379> BGSAVE

Background saving started
```

## 参考链接

[1]: https://blog.csdn.net/weixin_36755535/article/details/126958849	"【Docker系列】3.docker-compose安装redis"
[2]: https://www.runoob.com/redis/redis-tutorial.html	"菜鸟教程"

