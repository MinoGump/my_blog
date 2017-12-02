title: Ubuntu下安装Redis服务器
date: 2015-11-20 12:23:24
tags:
- Redis
categories:
- Tech
- Database
- Redis
---

# Ubuntu下安装Redis服务器

Redis是一个开源、支持网络、基于内存、键值对存储数据库，使用ANSIC编写。Redis是目前最流行的键值对存储数据库。

Redis的优点主要是具有非常丰富的数据结构（包括双向链表，set，hashmap等），其次还能高速读写，可以保证并发访问操作数据的一致性。

Redis的缺点主要是耗内存和写入磁盘的速度慢。因为Redis是将数据直接存储到内存中的，因此内存消耗很大，备份数据时间花费较大。

### Install Redis Server

- 下载Redis官方稳定版Server，可以[手动下载][redis_download]，也可以使用命令行下载
`# wget http://download.redis.io/releases/redis-3.0.5.tar.gz`

- 解压并移动到/opt文件夹中
`# tar xzf redis-3.0.5.tar.gz`
`# mv redis-3.0.5/ /opt/`

- 编译redis
`# cd /opt/redis-3.0.5`
`# make`

- 编译完后会出现"It's a good idea to run 'make test'"返回信息，于是我们make test看看测试效果如何
`# make test`

- make test完毕后如无意外能够返回"all tests passed without errors!"，用"make install"检查一下是否所有都安装成功
`# sudo make install`
![photo1](/img/redis/make-install.png)

- 启动redis服务器
`# cd utils/`
`# ./install_server.sh`
`# service redis_6379 start`
`# redis-cli`

- 关闭redis服务器
`# service redis_6379 stop`

### Install Python API

- 安装一个python的redis接口，使py文件可以连接到redis服务器
`$ sudo pip install redis`

- 在python中使用redis
`>>> import redis`
`>>> r = redis.StrictRedis(host="localhost", port=6379, db=0)`


### Usage of Redis in Python

- set
`>>> r.set('foo', 'bar')`
`>>> r.get('foo')`

- mset mget
`>>> r.mset(first = 10, second = 20, third = 30)`
`>>> r.mget("first", "second")`

- incr
`>>> r.set("counter", 100)`
`>>> r.get("conuter")`
`>>> r.incr("counter")`
`>>> r.incrby("counter", 10)`

- exist
`>>> r.exists("first")`
`>>> r.exists("firs")`

- del
`>>> r.delete("first")`
`>>> r.delete("firs")`
`>>> r.get("first")`

- expire (n秒后删除数据)
`>>> r.expire("second", 10)`

- list (双向链表，可用做队列和堆栈)
`>>> r.lpush("mylist", 1)`
`>>> r.lrange("mylist", 0, -1)`
`>>> r.lpush("mylist", 2, 3, 2, 3)`
`>>> r.lrange("mylist", 0, -1)`
`>>> r.rpush("mylist", 4)`
`>>> r.lrange("mylist", 0, -1)`

- hash map
`>>> r.hmset("user", {'name': 'robert', 'age': 32})`
`>>> r.hgetall("user")`
`>>> r.hget("user","name")`

- 更多用法参考[官方文档][python-redis]

### Reference

- https://www.youtube.com/watch?v=TtQEfbp_Ihc


[//]: *******************************************************************************************************
 [redis_download]: <http://redis.io/download>
 [python-redis]: <https://kushal.fedorapeople.org/redis-py/html/api.html>
