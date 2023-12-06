---
title: MySQL学习笔记
date: 2023.11.15 22:00:00
tags: [Mysql, 微服务, docker]
categories: 学习笔记

---

# MySQL学习笔记

## docker安装MySQL

### docker-compose.yml

```dockerfile
version: '3'
services:
  mysql:                                            #mysql服务节点
    image: mysql: 5.7.44                            #mysql镜像，如果镜像容器没有会去自动拉取
    container_name: mysql                           #容器的名称
    command:                                        #构建容器后所执行的命令
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_unicode_ci
      --lower-case-table-names=1    #忽略数据表明大小写 
    environment:
      MYSQL_ROOT_PASSWORD: 123456                     #设置root帐号密码
    ports:
      - 3306:3306
    volumes:
      - ./data:/var/lib/mysql           #数据文件挂载
      - ./conf.d:/etc/mysql/conf.d      #配置文件挂载
      - ./logs:/var/log/mysql            #日志文件挂载
```

### my.cnf(./conf.d/my.cnf)

```
# Copyright (c) 2017, Oracle and/or its affiliates. All rights reserved.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; version 2 of the License.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301 USA

#
# The MySQL  Server configuration file.
#
# For explanations see
# http://dev.mysql.com/doc/mysql/en/server-system-variables.html

# Custom config should go here

[mysqld]
#设置连接超时时间为21天
wait_timeout=1814400

#关闭binlog
skip-log-bin

#最大允许传输包的大小
max_allowed_packet=20M

#no case sensitive 这个仅mysql初始化时有效
lower_case_table_names=1

#no passwd
#skip-grant-tables

#最大连接数
max_connections=5000

[mysql]
host=localhost
user=root
password=123456
#设置mysql客户端默认字符集
default-character-set=utf8mb4

[mysqldump]
#防止mysqldump导出出现警告
host=localhost
user=root
password=123456

[client]
port=3306
default-character-set=utf8mb4
```

## 简介

## MySQL操作

### 启动MySQL

```
net start mysql
```

### 登入MySQL

```
mysql -u root -p
# 输入密码：
00000000
```

## 数据库操作

## 数据操作

## 进阶操作

### 正则表达式

## 高级操作

### 数据备份和恢复

## 参考链接

[1]: https://www.runoob.com/mysql/mysql-tutorial.html	"菜鸟教程"
[2]: https://tuonioooo-notebook.gitbook.io/docker/docker-compose/docker-compose%E5%AE%89%E8%A3%85mySql	"docker-compose安装mySql"

