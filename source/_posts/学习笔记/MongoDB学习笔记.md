---
title: MongDB学习笔记
tags: [mongodb, 数据库]
categories: 学习笔记
date: 2023-11-14 11:30
---

# MongoDB学习笔记

## 软件下载

### docker-compose

```dockerfile
version: "3.9"
services:
  mongodb:
    image: mongo:4.4
    container_name: mongodb
    environment:
      - TZ=Asia/Shanghai
      - MONGO_INITDB_DATABASE=mongodb  
      - MONGO_INITDB_ROOT_USERNAME=root
      - MONGO_INITDB_ROOT_PASSWORD=123456
    ports:
      - "27017:27017"
    volumes:
      - ./data:/data/db
      - ./logs:/data/logs
      - ./config:/data/configdb
      - ./conf/mongod.conf:/etc/mongo/mongod.conf
    command: mongod --config /etc/mongo/mongod.conf
```



#### MongoDB配置文件mongod.conf

```
systemLog:
  destination: file
  path: /data/logs/mongod.log
  logAppend: true
storage:
  dbPath: /data/db
  journal:
    enabled: true
  wiredTiger:
    engineConfig:
      cacheSizeGB: 1
  engine: wiredTiger
  directoryPerDB: true
net:
  bindIpAll: true
  port: 27017
  maxIncomingConnections: 10000
security:
  authorization: enabled
```

> 配置文件放在，`docker-compose.yml`当前文件夹的下面的`conf`文件夹

## MongoDB的CRUD

### 基础概念

| SQL术语/概念 | MongoDB术语/概念 |              解释/说明              |
| :----------: | :--------------: | :---------------------------------: |
|   database   |     database     |               数据库                |
|    table     |    collection    |            数据库表/集合            |
|     row      |     document     |           数据记录行/文档           |
|    column    |      field       |             数据字段/域             |
|    index     |      index       |                索引                 |
| table joins  |                  |        表连接,MongoDB不支持         |
| primary key  |   primary key    | 主键,MongoDB自动将_id字段设置为主键 |

### 数据库操作

#### 显示所有数据的列表

```
show dbs
```

1. 执行 `db` 命令可以显示当前数据库对象或集合。

2. 运行`use 数据库名`命令，可以连接到一个指定的数据库。

3. 数据库也通过名字来标识。数据库名可以是满足以下条件的任意UTF-8字符串。

   > 不能是空字符串（"")。
   >
   > 不得含有' '（空格)、.、$、/、\和\0 (空字符)。
   >
   > 应全部小写。
   >
   > 最多64字节。

#### 特殊的数据库

有一些数据库名是保留的，可以直接访问这些有特殊作用的数据库。

* **admin**： 从权限的角度来看，这是"root"数据库。要是将一个用户添加到这个数据库，这个用户自动继承所有数据库的权限。一些特定的服务器端命令也只能从这个数据库运行，比如列出所有的数据库或者关闭服务器。
* **local:** 这个数据永远不会被复制，可以用来存储限于本地单台服务器的任意集合
* **config**: 当Mongo用于分片设置时，config数据库在内部使用，用于保存分片的相关信息。

#### 创建数据库

```
use DATABASE_NAME
```

#### 删除数据库

```
db.dropDatabase()
```

> db表示当前数据库，所以命令的意思为删除当前数据库。

### 数据类型

|      数据类型      |                             描述                             |
| :----------------: | :----------------------------------------------------------: |
|       String       | 字符串。存储数据常用的数据类型。在 MongoDB 中，UTF-8 编码的字符串才是合法的。 |
|      Integer       | 整型数值。用于存储数值。根据你所采用的服务器，可分为 32 位或 64 位。 |
|      Boolean       |              布尔值。用于存储布尔值（真/假）。               |
|       Double       |                双精度浮点值。用于存储浮点值。                |
|    Min/Max keys    | 将一个值与 BSON（二进制的 JSON）元素的最低值和最高值相对比。 |
|       Array        |            用于将数组或列表或多个值存储为一个键。            |
|     Timestamp      |            时间戳。记录文档修改或添加的具体时间。            |
|       Object       |                        用于内嵌文档。                        |
|        Null        |                        用于创建空值。                        |
|       Symbol       | 符号。该数据类型基本上等同于字符串类型，但不同的是，它一般用于采用特殊符号类型的语言。 |
|        Date        | 日期时间。用 UNIX 时间格式来存储当前日期或时间。你可以指定自己的日期时间：创建 Date 对象，传入年月日信息。 |
|     Object ID      |                 对象 ID。用于创建文档的 ID。                 |
|    Binary Data     |               二进制数据。用于存储二进制数据。               |
|        Code        |         代码类型。用于在文档中存储 JavaScript 代码。         |
| Regular expression |                       正则表达式类型。                       |

### 集合操作

#### 创建集合

```
db.createCollection(name, options)
```

> 参数说明：
>
> * name: 要创建的集合名称，是要字符串形式的，就是要有双引号。
> * options: 可选参数, 指定有关内存大小及索引的选项

#### 删除集合

```
db.COLLECTION_NAME.drop()
```

### 文档操作

#### 查询文档

```sql
db.COLLECTION_NAME.find(query, projection)
```

> * **query** ：可选，使用查询操作符指定查询条件
> * **projection** ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。

如果你需要以易读的方式来读取数据，可以使用 pretty() 方法，语法格式如下：

```
>db.col.find().pretty()
```

pretty() 方法以格式化的方式来显示所有文档。

#### 插入文档

`db.COLLECTION_NAME.insertOne()` 用于向集合插入一个新文档，语法格式如下：

```
db.COLLECTION_NAME.insertOne( <document>)
```

`db.COLLECTION_NAME.insertMany()` 用于向集合插入一个多个文档，语法格式如下：

```
db.COLLECTION_NAME.insertMany([ 
   <document 1> , 
   <document 2>,
   ... 
])
```

> 参数说明：
>
> **document**：要写入的文档。

#### 更新文档

```
db.collection.update(
   <query>,
   <update>
)
```

> **参数说明：**
>
> * **query** : update的查询条件，类似sql update查询内where后面的。
> * **update** : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的

**例子**

```sql
// 更新所有年龄为25岁的John的城市修改为"Chicago"
db.exampleCollection.updateMany(
  { name: "John", age: 25 },
  { $set: { city: "Chicago" } }
)
```

> `$set`为操作符，更多操作符参考下文的[文档操作符](#operator_anchor)

#### 删除文档

`db.COLLECTION_NAME.deleteOne()` 用于向集合删除符合条件的多个文档，语法格式如下：

```
db.collection.deleteOne (<query>)
```

db.collection.deleteOne() 用于向集合删除符合条件的多个文档，语法格式如下：

```
db.collection.deleteMany (<query>)
```



> **query** :（可选）删除的文档的条件。

## 数据库的其他操作

### 文档操作符{#operator_anchor}

| 操作符      | 描述                             | 示例                                                   |
| ----------- | -------------------------------- | ------------------------------------------------------ |
| `$set`      | 设置文档中的字段的值或添加新字段 | `{ $set: { "newField": "someValue" } }`                |
| `$unset`    | 从文档中删除指定字段             | `{ $unset: { "fieldToRemove": "" } }`                  |
| `$inc`      | 递增（或递减）字段的值           | `{ $inc: { "numericField": 5 } }`                      |
| `$push`     | 将值添加到数组字段               | `{ $push: { "arrayField": "newValue" } }`              |
| `$pull`     | 从数组字段中移除指定值           | `{ $pull: { "arrayField": "valueToRemove" } }`         |
| `$rename`   | 重命名字段                       | `{ $rename: { "oldField": "newField" } }`              |
| `$addToSet` | 仅在数组字段中不存在时添加值     | `{ $addToSet: { "uniqueArrayField": "uniqueValue" } }` |

### 条件操作符

| 操作       | 格式                     | 范例                                        | RDBMS中的类似语句       |
| :--------- | :----------------------- | :------------------------------------------ | :---------------------- |
| 等于       | `{<key>:<value>`}        | `db.col.find({"by":"菜鸟教程"}).pretty()`   | `where by = '菜鸟教程'` |
| 小于       | `{<key>:{$lt:<value>}}`  | `db.col.find({"likes":{$lt:50}}).pretty()`  | `where likes < 50`      |
| 小于或等于 | `{<key>:{$lte:<value>}}` | `db.col.find({"likes":{$lte:50}}).pretty()` | `where likes <= 50`     |
| 大于       | `{<key>:{$gt:<value>}}`  | `db.col.find({"likes":{$gt:50}}).pretty()`  | `where likes > 50`      |
| 大于或等于 | `{<key>:{$gte:<value>}}` | `db.col.find({"likes":{$gte:50}}).pretty()` | `where likes >= 50`     |
| 不等于     | `{<key>:{$ne:<value>}}`  | `db.col.find({"likes":{$ne:50}}).pretty()`  | `where likes != 50`     |

### 逻辑操作符

#### AND 条件

MongoDB 的 find() 方法可以传入多个键(key)，每个键(key)以逗号隔开，即常规 SQL 的 AND 条件。

语法格式如下：

```
db.COLLECTION_NAME.find({key1:value1, key2:value2}).pretty()
```

####  OR 条件

MongoDB OR 条件语句使用了关键字 **$or**,语法格式如下：

```
db.COLLECTION_NAME.find(
   {
      $or: [
         {key1: value1}, {key2:value2}
      ]
   }
).pretty()
```

### MongoDB Limit与Skip方法

如果你需要在MongoDB中读取指定数量的数据记录，可以使用MongoDB的Limit方法，limit()方法接受一个数字参数，该参数指定从MongoDB中读取的记录条数。

limit()方法基本语法如下所示：

```
db.COLLECTION_NAME.find().limit(NUMBER)
```

#### MongoDB Skip() 方法

我们除了可以使用limit()方法来读取指定数量的数据外，还可以使用skip()方法来跳过指定数量的数据，skip方法同样接受一个数字参数作为跳过的记录条数。

skip() 方法脚本语法格式如下：

```
db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)
```

### 排序

#### MongoDB sort() 方法

在 MongoDB 中使用 sort() 方法对数据进行排序，sort() 方法可以通过参数指定排序的字段，并使用 1 和 -1 来指定排序的方式，其中 1 为升序排列，而 -1 是用于降序排列。

sort()方法基本语法如下所示：

```
db.COLLECTION_NAME.find().sort({KEY:1})
```

### 索引

索引通常能够极大的提高查询的效率，如果没有索引，MongoDB在读取数据时必须扫描集合中的每个文件并选取那些符合查询条件的记录。

这种扫描全集合的查询效率是非常低的，特别在处理大量的数据时，查询可以要花费几十秒甚至几分钟，这对网站的性能是非常致命的。

索引是特殊的数据结构，索引存储在一个易于遍历读取的数据集合中，索引是对数据库表中一列或多列的值进行排序的一种结构

#### createIndex() 方法

MongoDB使用 createIndex() 方法来创建索引。

createIndex()方法基本语法格式如下所示：

```
db.COLLECTION_NAME.createIndex(keys, options)
```

语法中 Key 值为你要创建的索引字段，1 为指定按升序创建索引，如果你想按降序来创建索引指定为 -1 即可。

**例子**

```
db.exampleCollection.createIndex({ name: 1, age: -1 })
```



### 聚合函数

| 表达式    | 描述                                                         | 实例                                                         |
| :-------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| $sum      | 计算总和。                                                   | db.COLLECTION_NAME.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : "$likes"}}}]) |
| $avg      | 计算平均值                                                   | db.COLLECTION_NAME.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}]) |
| $min      | 获取集合中所有文档对应值得最小值。                           | db.COLLECTION_NAME.aggregate([{$group : {_id : "$by_user", num_tutorial : {$min : "$likes"}}}]) |
| $max      | 获取集合中所有文档对应值得最大值。                           | db.COLLECTION_NAME.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}]) |
| $push     | 将值加入一个数组中，不会判断是否有重复的值。                 | db.COLLECTION_NAME.aggregate([{$group : {_id : "$by_user", url : {$push: "$url"}}}]) |
| $addToSet | 将值加入一个数组中，会判断是否有重复的值，若相同的值在数组中已经存在了，则不加入。 | db.COLLECTION_NAME.aggregate([{$group : {_id : "$by_user", url : {$addToSet : "$url"}}}]) |
| $first    | 根据资源文档的排序获取第一个文档数据。                       | db.COLLECTION_NAME.aggregate([{$group : {_id : "$by_user", first_url : {$first : "$url"}}}]) |
| $last     | 根据资源文档的排序获取最后一个文档数据                       | db.COLLECTION_NAME.aggregate([{$group : {_id : "$by_user", last_url : {$last : "$url"}}}]) |

> 注意： `$group`可以根据指定的条件将文档分组，`$first` 获取第一个文档数据，进而就可以进行数值上的操作。

## 高级操作

### 正则表达式

以下命令使用正则表达式查找包含 runoob 字符串的文章：

```
>db.COLLECTOION_NAME.find({post_text:{$regex:"runoob"}})
```

以上查询也可以写为：

```
>db.COLLECTOION_NAME.find({post_text:/runoob/})
```

### SQL脚本

这是测试用例所用到的数据

```
use test
db.testCollection.insertMany([
  { name: "Elijah", age: 29, city: "Dallas" },
  { name: "Charlotte", age: 27, city: "Raleigh" },
  { name: "Henry", age: 31, city: "Nashville" },
  { name: "Luna", age: 26, city: "Orlando" },
  { name: "Alexander", age: 33, city: "Minneapolis" },
  { name: "Aria", age: 28, city: "Detroit" },
  { name: "James", age: 30, city: "Philadelphia" },
  { name: "Scarlett", age: 25, city: "Memphis" },
  { name: "Logan", age: 32, city: "Salt Lake City" },
  { name: "Zoe", age: 29, city: "New Orleans" },
  { name: "Jackson", age: 34, city: "San Antonio" },
  { name: "Penelope", age: 27, city: "Denver" },
  { name: "Carter", age: 31, city: "Kansas City" },
  { name: "Grace", age: 28, city: "San Francisco" },
  { name: "Julian", age: 30, city: "Charlotte" },
  { name: "Stella", age: 26, city: "Portland" },
  { name: "Leo", age: 32, city: "Indianapolis" },
  { name: "Mila", age: 29, city: "Austin" },
  { name: "Ezra", age: 33, city: "Seattle" },
  { name: "Hazel", age: 27, city: "Phoenix" },
  { name: "Grace", age: 27, city: "Boston" },
  { name: "Oliver", age: 31, city: "Austin" },
  { name: "Emma", age: 29, city: "Portland" },
  { name: "William", age: 33, city: "Phoenix" },
  { name: "Ava", age: 26, city: "San Diego" },
  { name: "Liam", age: 30, city: "Houston" },
  { name: "Mia", age: 24, city: "Atlanta" },
  { name: "Benjamin", age: 34, city: "Las Vegas" },
  { name: "Amelia", age: 28, city: "Chicago" },
  { name: "Lucas", age: 32, city: "Seattle" },
  { name: "Eva", age: 22, city: "Seattle" },
  { name: "Michael", age: 35, city: "Chicago" },
  { name: "Sophia", age: 28, city: "Miami" },
  { name: "Daniel", age: 32, city: "Denver" }
])
```



### 数据库的备份和恢复

在Mongodb中我们使用mongodump命令来备份MongoDB数据。该命令可以导出所有数据到指定目录中。

mongodump命令可以通过参数指定导出的数据量级转存的服务器。

mongodump命令脚本语法如下：

```
mongodump -h dbhost -d dbname -o dbdirectory
```

> * -h：MongoDB 所在服务器地址，例如：127.0.0.1，当然也可以指定端口号：127.0.0.1:27017
>
> * -d：需要备份的数据库实例，例如：test
>
> * -o：备份的数据存放位置，例如：c:\data\dump，当然该目录需要提前建立，在备份完成后，系统自动在dump目录下建立一个test目录，这个目录里面存放该数据库实例的备份数据。

## 参考链接

[1]: https://www.runoob.com/mongodb/mongodb-tutorial.html	"菜鸟教程 MongoDB 教程"

[2]: https://blog.csdn.net/weixin_42225792/article/details/131292784	"Docker-compose 安装 redis"

