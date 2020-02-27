# Redis

![Redis](https://redis.io/images/redis-white.png)

[Redis](https://redis.io/)是一个开源的使用ANSI  C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。从2010年3月15日起，Redis的开发工作由VMware主持。从2013年5月开始，Redis的开发由Pivotal赞助。[中文官网](http://Redis.cn)

Redis是 NoSQL技术阵营中的一员，它通过多种键值数据类型来适应不同场景下的存储需求，借助一些高层级的接口使用其可以胜任，如缓存、队列系统的不同角色。

**Redis 与其他 key-value 缓存产品有以下三个特点**：

+ Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
+ Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
+ Redis支持数据的备份，即master-slave模式的数据备份。

**Redis的优势**:

+ 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
+ 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
+ 原子 – Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行。
+ 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。

**Redis的应用场景**:
+ 用来做缓存(ehcache/memcached)——redis的所有数据是放在内存中的（内存数据库）
+ 可以在某些特定应用场景下替代传统数据库——比如社交类的应用
+ 在一些大型系统中，巧妙地实现一些特定的功能：session共享、购物车
+ 只要你有丰富的想象力，redis可以用在可以给你无限的惊喜…….



Redis具有丰富的客户端，支持现阶段流行的大多数编程语言。



## 快速入门

### 安装、配置、启动与测试

使用如下命令安装：

```bash
$ sudo apt-get install redis
```

安装完成后包含如下工具：

|     工　具      |     用　途      |
| :-------------: | :-------------: |
|  redis-server   |   redis服务器   |
| redis-cli redis |  命令行客户端   |
| redis-benchmark |  性能测试工具   |
| edis-check-aof  | AOF文件修复工具 |
| redis-check-rdb | RDB文件检索工具 |

系统配置文件在`/etc/redis/redis.conf`。

**核心配置选项**:
+ 绑定ip：如果需要远程访问，可将此⾏注释，或绑定⼀个真实IP
	```bash
	bind 127.0.0.1
	port 6379
	```
+ 是否以守护进程运⾏：如果以守护进程运⾏，则不会在命令⾏阻塞，类似于服务；如果以⾮守护进程运⾏，则当前终端被阻塞；设置为yes表示守护进程，设置为no表示⾮守护进程；推荐设置为yes
	```bash
	daemonize yes
	```
+ 数据⽂件存储路径
	```bash
	dir /var/lib/redis
	​```bash
	```
+ ⽇志⽂件
	```bash
	logfile /var/log/redis/redis-server.log
	```
+ 数据库，默认有16个
	```bash
	database 16
	```
+ 主从复制，类似于双机备份
	```bash
	slaveof
	```

请使用如下命令控制redis服务：

```bash
$ sudo systemctl start redis # 启动redis服务
$ sudo systemctl status redis　# 查看redis服务的运行状态
$ sudo systemctl stop redis # 关闭服务
```

使用`redis-cli`命令与其服务进行交互：

```bash
$ redis-cli
redis-cli
127.0.0.1:6379> ping # 运行测试命令，返回PONG表示正常
PONG
127.0.0.1:6379>
```

数据库没有名称，默认有16个，通过0-15来标识，连接redis默认选择第一个数据库：

```
select n
```

### 数据库操作

redis是`key-value`的数据结构，每条数据都是⼀个键值对，键的类型是字符串，不能重复值的类型有String,List,Hash,Set等。

#### String

字符串类型是Redis中最为基础的数据存储类型，该类型可以接受任何格式的数据，如JPEG图像数据或Json对象描述信息等。在Redis中字符串类型的Value最多可以容纳的数据长度是512M。

设置键值，如果设置的键不存在则为添加，如果设置的键已经存在则修改:

```bash
set key value
get key
```

例如：

```bash
127.0.0.1:6379> set name "luo xiangyong"
OK
127.0.0.1:6379> get name
"luo xiangyong"
```

设置键值及过期时间，以秒为单位:

```bash
setex key seconds value
```

例如：

```bash
127.0.0.1:6379> setex tmp-data 10 "随便"
OK
127.0.0.1:6379> get tmp-data
"\xe9\x9a\x8f\xe4\xbe\xbf"
127.0.0.1:6379> get tmp-data
(nil)
```

设置多个键值

```bash
 mset key1 value1 key2 value2 ...
```

例如：

```bash
127.0.0.1:6379> mset a1 go a2 c++ a3 c
OK
127.0.0.1:6379> mget a1 a2 a3
1) "go"
2) "c++"
3) "c"
```

追加值

```
append key value
1
```

•	例4：向键为a1中追加值’ 真棒’

```
127.0.0.1:6379> append 'a1' '真棒'
(integer) 8
127.0.0.1:6379> get a1
"go\xe7\x9c\x9f\xe6\xa3\x92"
127.0.0.1:6379> 
```

我们发现上面输入的中文是乱码的，请退出redis-cli，采用下面的方式进入：

```bash
$ redis-cli --raw
127.0.0.1:6379> get a1
go真棒
127.0.0.1:6379>
```

查找键，参数⽀持正则表达式

```bash
keys pattern
```

例如，查找所有键：

```bash
127.0.0.1:6379> keys *
a1
name
00000010-00000000-00000004
a3
_kombu.binding.celery
a2
127.0.0.1:6379>
```

查看名称中包含a的键:

```bash
127.0.0.1:6379> keys a*
a1
a3
a2
127.0.0.1:6379>
```

判断键是否存在，如果存在返回1，不存在返回0

```
exists key
```

例如，判断键a1是否存在：

```bash 
127.0.0.1:6379> exists luoxiangyong
0
127.0.0.1:6379> exists a1
1
127.0.0.1:6379>
```

查看键对应的value的类型，返回为redis⽀持的五种类型中的⼀种：

```
type key
```

例如：

```bash
127.0.0.1:6379> type name
string
127.0.0.1:6379> type a1
string
127.0.0.1:6379>
```

删除键及对应的值

```bash
del key1 key2 ...
```

例如，删除`a1 a2`：

```bash
127.0.0.1:6379> del a1 a2
2
127.0.0.1:6379> 
```

设置过期时间，以秒为单位，如果没有指定过期时间则⼀直存在，直到使⽤DEL移除：

```
expire key seconds
```

查看有效时间，以秒为单位：

```bash
ttl key
```

例如：

```bash
127.0.0.1:6379> ttl name
-1						# 永久保存，直到删除
127.0.0.1:6379> set b 100
OK
127.0.0.1:6379> type b
string
127.0.0.1:6379> expire b 1000
1
127.0.0.1:6379> ttl b
997
127.0.0.1:6379> 
```

#### hash

hash⽤于存储对象，对象的结构为属性、值，值的类型为string。

设置单个属性

```bash
hset key field value
```

例，设置键 user的属性name为"罗祥勇"

```bash
127.0.0.1:6379> hset user name 罗祥勇
1
```

设置多个属性

```bash
hmset key field1 value1 field2 value2 ...
```

获取指定键所有的属性

```
hkeys key
```

获取⼀个属性的值

```bash
hget key field
```

获取多个属性的值

```bash
hmget key field1 field2 ...
```

例如：

```bash
127.0.0.1:6379> hkeys user
name
127.0.0.1:6379> hget user name
罗祥勇
127.0.0.1:6379> 
```

获取所有属性的值:

```
hvals key
```

获取一个hash有多少个属性：

```
hlen key
```

删除整个hash键及值，使⽤del命令，删除属性，属性对应的值会被⼀起删除：

```
hdel key field1 field2 ...
```

#### list

列表的元素类型为string，按照插⼊顺序排序保存。
 在左侧插⼊数据：

```
lpush key value1 value2 ...
```

例如：

```bash
127.0.0.1:6379> lpush a1 1 2 3
3
127.0.0.1:6379> lpush a1 1 2 3
6
127.0.0.1:6379> lrange a1 0 3
3
2
1
3
127.0.0.1:6379>
```

​	在右侧插⼊数据

```bash
rpush key value1 value2 ...
```

​	在指定元素的前或后插⼊新元素

```bash
linsert key before或after 现有元素 新元素
```

返回列表⾥指定范围内的元素，start、stop为元素的下标索引，索引从左侧开始，第⼀个元素为0，索引可以是负数，表示从尾部开始计数，如-1表示最后⼀个元素:

```bash
lrange key start stop
```

设置指定索引位置的元素值，索引从左侧开始，第⼀个元素为0，索引可以是负数，表示尾部开始计数，如-1表示最后⼀个元素:

```bash
lset key index value
```

删除指定元素，将列表中前`count`次出现的值为`value`的元素移除，`count > 0:` 从头往尾移除， `count < 0:` 从尾往头移除， `count = 0:` 移除所有:

```bash
lrem key count value
```

#### set

set是⽆序集合，元素为string类型，具有唯⼀性，不重复，对于集合没有修改操作。

添加元素：

```
sadd key member1 member2 ...
```

获取，返回所有的元素：

```
smembers key
```

删除指定元素：

```
srem key value
```

#### zset

zset表示sorted set，有序集合，元素为string类型，元素具有唯⼀性，不重复。每个元素都会关联⼀个double类型的score，表示权重，通过权重将元素从⼩到⼤排序。本类型没有修改操作。

添加:

```
zadd key score1 member1 score2 member2 ...
```

获取，返回指定范围内的元素，start、stop为元素的下标索引，索引从左侧开始，第⼀个元素为0，索引可以是负数，表示从尾部开始计数，如-1表示最后⼀个元素：

```
zrange key start stop
```

返回score值在min和max之间的成员：

```
zrangebyscore key min max
```

返回成员member的score值：

```
zscore key member
```

删除指定元素：

```
zrem key member1 member2 ...
```

删除权重在指定范围的元素

```
zremrangebyscore key min max
```



## Redis编程

好了，上面费了老鼻子劲介绍Redis操作的知识，现在终于可以开始我们的编程了。

### 准备实验数据

我们就以要编写的程序`gprl-redis-demo`项目`Cargo.toml`中的`package`组中的字段信息建立hash吧：

```bash
$ redis-cli --raw
127.0.0.1:6379> hset gprl-redis-demo name gprl-redis-demo
1
127.0.0.1:6379> hset gprl-redis-demo version 0.1.0
1
127.0.0.1:6379> hset gprl-redis-demo edition 2018
1
127.0.0.1:6379> hset gprl-redis-demo authors "Luo Xiangyong <luoxiangyong@topgridcloud.com>"
1
127.0.0.1:6379> hvals gprl-redis-demo
gprl-redis-demo
0.1.0
2018
Luo Xiangyong <luoxiangyong@topgridcloud.com>
127.0.0.1:6379> hkeys gprl-redis-demo
name
version
edition
authors
127.0.0.1:6379> 
```

### 开始编码

redis crate是redis客户端的Rust语言实现。她提供了与redis交互的一般接口，同时还提供了一些方面使用的辅助功能。



这个库提供两种级别的API：low-level和high-level。 High-level的API的功能目前还是太全面。Low-level可以用来控制所有与redis的交互。编程时，我们可以随便在这两种方式中切换。



这个库提供了三种方式的连接参数：

```bash
redis://[:<passwd>@]<hostname>[:port][/<db>]
redis+unix:///[:<passwd>@]<path>[?db=<db>]
unix:///[:<passwd>@]<path>[?db=<db>]
```



使用low-level级别的API的话，你可以使用`cmd`函数，该函数可以创建redis请求。创建好命令对象后就使用`ConnectionLike`类型的对象为参数调用`query`函数建立请求了。比如：

```rust
fn do_something(con: &mut redis::Connection) -> redis::RedisResult<()> {
    let _ : () = redis::cmd("SET").arg("my_key").arg(42).query(con)?;
    Ok(())
}
```

上面的例子返回一个类`Result<T>`的对象。如果你不关心返回值，可以使用`()`类型注释其类型。



使用high-level级别的API是类似的。使用`redis::Commands`trait就可以了，`ConnectionLike`的对象都实现了这个trait，这样编码就比较简单了。比如：

```rust
extern crate redis;
use redis::Commands;

fn do_something(con: &mut redis::Connection) -> redis::RedisResult<()> {
    let _ : () = con.set("my_key", 42)?;
    Ok(())
}
```

不过请注意，high-level级别的API还在开发，很多功能还缺失。



有时候一个查询语句会返回多个值，我们可以利用迭代器访问查询结果，比如：

```rust
let mut iter : redis::Iter<isize> = redis::cmd("SSCAN").arg("my_set")
    .cursor_arg(0).clone().iter(&mut con)?;
for x in iter {
    // do something with the item
}
```





这个库还提供了pipeline功能，可以一次发送多个请求，比如：

```rust
let (k1, k2) : (i32, i32) = redis::pipe()
    .cmd("SET").arg("key_1").arg(42).ignore()
    .cmd("SET").arg("key_2").arg(43).ignore()
    .cmd("GET").arg("key_1")
    .cmd("GET").arg("key_2").query(&mut con)?;
```

如果想原子操作pipeline，可以如下操作：

```rust
let (k1, k2) : (i32, i32) = redis::pipe()
    .atomic()
    .cmd("SET").arg("key_1").arg(42).ignore()
    .cmd("SET").arg("key_2").arg(43).ignore()
    .cmd("GET").arg("key_1")
    .cmd("GET").arg("key_2").query(&mut con)?;
```

也可以使用pipeline执行high-level API，比如：

```rust
let (k1, k2) : (i32, i32) = redis::pipe()
    .atomic()
    .set("key_1", 42).ignore()
    .set("key_2", 43).ignore()
    .get("key_1")
    .get("key_2").query(&mut con)?;
```

另外，该库还支持发布/订阅、事务、脚本、异步等功能。详细请参考官方文档：https://docs.rs/redis。



好了，节本解释就这些，开始撸码吧。

```bash
$ mkdir chapter05
$ cd chapter05
$ cargo new --bin gprl-redis-demo
$ cd gprl-redis-demo
```

`Cargo.toml`文件内容如下：

```toml
[package]
name = "gprl-redis-demo"
version = "0.1.0"
authors = ["Luo Xiangyong <luoxiangyong@topgridcloud.com>"]
edition = "2018"

[dependencies]
redis = "*"
```

`src/main.rs`文件内容如下：

```rust
extern crate redis;

use redis::Commands;

use std::collections::HashMap;

fn main() {
    let client = redis::Client::open("redis://127.0.0.1/").unwrap();
    let mut con = client.get_connection().unwrap();
    
    let map : HashMap<String, String> = con.hgetall("gprl-redis-demo").unwrap();
    
    println!("修改前的gprl-redis-demo:\n{:?}", map);
    let () = con.hset("gprl-redis-demo","authors", "罗祥勇 <solo_lxy@126.com>").unwrap();
    
    let map : HashMap<String, String> = con.hgetall("gprl-redis-demo").unwrap();
    println!("修改后的gprl-redis-demo:\n{:?}", map);
}
```

运行效果如下：

```bash
$ cargo run
   Compiling gprl-redis-demo v0.1.0 (/home/luoxiangyong/writing-book/rust-in-practice-code/chapter05/gprl-redis-demo)
    Finished dev [unoptimized + debuginfo] target(s) in 1.27s
     Running `target/debug/gprl-redis-demo`
修改前的gprl-redis-demo:
{"name": "gprl-redis-demo", "edition": "2018", "authors": "Luo Xiangyong <luoxiangyong@topgridcloud.com>", "version": "0.1.0"}
修改后的gprl-redis-demo:
{"authors": "罗祥勇 <solo_lxy@126.com>", "version": "0.1.0", "edition": "2018", "name": "gprl-redis-demo"}
```

我们首先显示刚才准备的数据，然后修改该hash的`authors`字段为"罗祥勇 <solo_lxy@126.com>"。有了以上的解释，代码本省就很简单了，有一点需要注意，第14行的代码一定要写上返回变量的类型！



