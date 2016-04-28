Redis
远程服务器字典

homebrew 安装的

```To have launchd start redis now and restart at login:
  brew services start redis
Or, if you don't want/need a background service you can just run:
  redis-server /usr/local/etc/redis.conf
```

##bin目录可执行程序
redis-server Redis服务器
redis-cli 客户端
redis-benchmark 性能测试工具
redis-check-aof AOF文件修复工具
redis-check-dump RDB文件检测工具
redis.conf Redis的配置文件

将配置文件的daemonize
daemonize = yes 守护进程启用  

#返回值
##状态回复
Ping
SET test 'this is jankz test'
SET test1 'maizi1'

#错误回复
TESTERROR
##以interger回复
DBSIZE
##以字符串回复
GET test
GET test1
(nil) 代表空的结果
##多行字符串回复
KEYS *

```jankz:~ jankz$ redis-cli
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> SET test 'This is jankz test'
OK
127.0.0.1:6379> GET test
"This is jankz test"
127.0.0.1:6379> DBSIZE
(integer) 1
127.0.0.1:6379> SET test2 'Maizi'
OK
127.0.0.1:6379> GET test2
"Maizi"
127.0.0.1:6379> GETERROR
(error) ERR unknown command 'GETERROR'
127.0.0.1:6379> 

127.0.0.1:6379> KEYS *
1) "test"
2) "test2"
```
#配置相关
启动时指定配置文件
shotdown关闭
CONFIG GET loglevel

```.
1) "loglevel"
2) "notice"
```
CONFIG GET port

```.
1) "port"
2) "6379"
```
##动态设置配置属性
CONFIG SET name value
这里指定会覆盖默认的值

##配置选项 redis.conf
----连接选项---
###port 端口号
###bind 127.0.0.1 默认绑定的主机地址
###timeout 0 客户端闲置多久后关闭连接 0代表没有启用选项
###loglevel 日志级别 默认notice
debug  很详细信息 适合开发测试
verbose 包含很多不太有用的信息
notice 比较适合生产环境
warning  警告信息
###logfile  默认stdout 日志记录方式 标准输

###database 16, 默认数据库的数量16个
----和快照相关
## save
save<seconds><changes>;多少秒有多少更改将其同步到磁盘文件
save 900 1
save 300 10
save 60 10000
##rdbcompression yes 存储本地数据库是否压缩 默认yes

##dbfilename 指定本地数据库的文件名 默认dump.rdb

##dir 指定本地数据库的存放目录 默认当前目录



#数据类型
##String类型
一个键最多存储512M
###SET : 设置key 对应的值为value
语法 SET KEY VALUE [EX seconds][PX milliseconds]

EX seconds 设置键的key的过期时间
PX milliseconds :以毫秒的形式设置过期时间 SET key value PX milliseconds-PSETEX
NX 只有键不存在的时候才可以设置成功SET key value NX -SETNX
XX 只有key已经存在的时候才可以设置
可以自由组合

## #计数相关
####INCR KEY  对key中存储的数字加1
SET counter 1
INCR counter
key如果不存在会初始化为0再进行INCR操作
#### INCRBY  num 对key中存储的数字加num INCRBY不带空格
SET counter2 10
INCR BY counter2 16

```127.0.0.1:6379> INCR BY counter2 10
(error) ERR wrong number of arguments for 'incr' command
127.0.0.1:6379> INCRBY counter2 10
(integer) 20
127.0.0.1:6379>  

```
####INCRBYFLOAT 加浮点数
SET float1 4.2
INCRBYFLOAT float1 3.4

```(error) ERR syntax error
127.0.0.1:6379>  SET float1 4.2
OK
127.0.0.1:6379> INCRBYFLOAT float1 3.4
"7.6"
```
####DECR 将key中存储数字减1
DECR counter2
同名会覆盖
####DECRBY J将key中存储的数值减去指定的值

```127.0.0.1:6379> DECR counter2
(integer) 19
127.0.0.1:6379> DECRBY counter2 12 
(integer) 7

```
###GET 根据KEY 找到值
GET key 

SET testStr 'jankz'
GET testStr
SET testStr 'jankz1'
GET testStr 

不是字符串类型会报错
LPUSH testList1 a b c;
GET testList1

```127.0.0.1:6379> SET testStr  = 'jankz'
(error) ERR syntax error
127.0.0.1:6379> SET testStr 'jankz'
OK
127.0.0.1:6379> GET testStr
"jankz"
127.0.0.1:6379> SET testStr 'jankz1'
OK
127.0.0.1:6379> GET testStr
"jankz1"
127.0.0.1:6379> LPUSH testList1 a b c
(integer) 3
127.0.0.1:6379> GET testList1
(error) WRONGTYPE Operation against a key holding the wrong kind of value
127.0.0.1:6379> 
```
###GETRANGE key start end
下标从零开始
GETRANGE testStr2 0 4 
GETRANGE testStr2 0 -3
GETRANGE testStr2 -4 -2
GETRANGE testStr2 0 1000
GETRANGE testStr2 -4 2

```127.0.0.1:6379> SET testStr2 'maiziedu'
OK
127.0.0.1:6379> GETRANGE testStr2 0 4 
"maizi"
127.0.0.1:6379> GETRANGE testStr2 0 -3
"maizie"
127.0.0.1:6379> GETRANGE testStr2 -4 -2
"ied"
127.0.0.1:6379> GETRANGE testStr2 0 1000
"maiziedu"
127.0.0.1:6379> GETRANGE testStr2 -4 2
""
```
 ##GETSET 设置指定key的值
 SET testStr3 'King' 
 GET testStr3
 SET testStr3 'Queen' 
 GETSET testStr3 'King'
 
```127.0.0.1:6379> SET testStr3 'King'
OK
127.0.0.1:6379> GET testStr3
"King"
127.0.0.1:6379> SET testStr3 'Queen'
OK
127.0.0.1:6379> GETSET testStr3 'King'
"Queen"
127.0.0.1:6379> 
```
SET mycount =1
INCR mycount
INCR mycount

INCR mycount

INCR mycount
GET mycount
GETSET mycount 0


GETSET testStr4 'THIS IS A TEST'
GET testStr4

注意:
当key不存在 返回nil
如果key不是字符串会报错
###MSET 一次设置多个键
MSET testStr5 'KING5' testStr6 'KING6'
###MGET 一次获得多个键值
MGET testStr5  testStr6 
###STRLEN key 
返回字符串长度
###SETRANGE 替换字符串
SETRANGE key offset value
注意:如果设置的key原来的字符串长度比偏移量小 就会以零字节(\x00)来填充

SET testStr9 'Hello king'

SETRANGE testStr9 6 'QUEEN'

EXISTS 检测是否存在
SETRANGE testStr10 
SETRANGE testStr10 6 'King'

GETRANGE 返回字符串的一部分
语法 GETRANGE key start end

```127.0.0.1:6379> SET testStr9 'Hello Jankz'
OK
127.0.0.1:6379> GET testStr9
"Hello Jankz"
127.0.0.1:6379> SETRANGE testStr9 6 'Queen'
(integer) 11
127.0.0.1:6379> GET testStr9
"Hello Queen"
127.0.0.1:6379> SETRANGE testStr10 6 'Jankz'
(integer) 11
127.0.0.1:6379> GET testStr10
"\x00\x00\x00\x00\x00\x00Jankz"
127.0.0.1:6379> SETNX testStr10 'ajax'
(integer) 0
127.0.0.1:6379> SETNX testStr11 'ajax'
(integer) 1
127.0.0.1:6379> 
```
###SETNX 只有key 不存在才成功;
SETNX key value
###SETEX 
SETEX key seconds value
SETEX expireStr 60 'testExpire'
TTL testExpire

###MSETNX 只有所有的key不存在 才能成功.
###MGET
###PSETEX 以毫秒为单位设置key的生存周期
PSETEX test16 2000 ''hello'
PTTL
### APPEND 将值追加到字符串末尾
注意:如果key不存在 相当于执行SET 操作
APPEND testStr11 'edu'
APPEND noExistsTest 'ajax'

```127.0.0.1:6379> APPEND testStr11 'edu'
(integer) 7
127.0.0.1:6379> 
127.0.0.1:6379> GET testStr11
"ajaxedu"
127.0.0.1:6379> APPEND noExistsTest 'ajax'
(integer) 4
127.0.0.1:6379> GET noExistsTest
"ajax"
```
##Hash类型

###HSET 将哈希表key中域field设置成指定value
HSET key FIELD value
HSET userInfo1 username 'king'
HSET userInfo1 password '123456'
HSET userInfo1 email 'jankz@jankz.com'

```127.0.0.1:6379> APPEND noExistsTest 'ajax'
(integer) 4
127.0.0.1:6379> GET noExistsTest
"ajax"
127.0.0.1:6379> HSET userInfo1 username 'king'
(integer) 1
127.0.0.1:6379> HSET userInfo1 password '123456'
(integer) 1
127.0.0.1:6379> HSET userInfo1 email 'jankz@jankz.com'
(integer) 1
127.0.0.1:6379> EXISTS userInfo1
(integer) 1
```
注意:
如果哈希表中的Field 不存在 相当于新建Field 设置成功返回1
如果哈希表key中Field 存在 相当于重新赋值 成功返回0
 
###HGET key Field 
HGET userInfo1 username
HGET userInfo1 password
HGET userInfo1 email

```127.0.0.1:6379> HGET userInfo1 username
"king"
127.0.0.1:6379> HGET userInfo1 password
"123456"
127.0.0.1:6379> HGET userInfo1 email
"jankz@jankz.com"
```
注意:
如果key中field 不存在 返回是nil;
### HSETNX 将哈希表中的域Field设置成指定的值 只有field不存在才能成功
如果field 存在 操作无效 操作无效返回0
语法: HSETNX testhash1 test 'a'

```127.0.0.1:6379> HSETNX testhash1 test 'a'
(integer) 1
127.0.0.1:6379> HSETNX testhash1 test '111'
(integer) 0
127.0.0.1:6379> HGET testhash1
(error) ERR wrong number of arguments for 'hget' command
127.0.0.1:6379> HGET testhash1 test
"a"
```
###HMSET 通过将多个Field-value设置到hash表key
语法: HMSET key field value field value...
HMSET userInfo2 username 'jankz' nickname 'jz' password '123456' email 'jankz@jankz.com'
HGET userInfo2 username

HMSET userInfo2 username 'jankz2016' nickname 'admin' password '123456' email 'jankz@jankz.com'

###HMGET  一次获得哈希表中多个field的值
HMGET userInfo2 username nickname password email

```OK
127.0.0.1:6379> HMGET userInfo2 username nickname password email
1) "jankz"
2) "jz"
3) "123456"
4) "jankz@jankz.com"
```

HMGET userInfo3 a b c

```127.0.0.1:6379> HMGET userInfo3 a b c
1) (nil)
2) (nil)
3) (nil)
```
注意:如果哈希表key中field不存在会返回nil

###HGETALL 返回哈希表key中所有的field和value
语法HGETALL key

```127.0.0.1:6379> HGETALL userInfo2
1) "username"
2) "jankz"
3) "nickname"
4) "jz"
5) "password"
6) "123456"
7) "email"
8) "jankz@jankz.com"
127.0.0.1:6379> 
```
###HKEYS key 
HKEYS userInfo2

```127.0.0.1:6379> HKEYS userInfo2
1) "username"
2) "nickname"
3) "password"
4) "email"
```
###HVALS key 返回hash中key中所有的对应的值
语法 HVALS KEY 
HVALS userInfo2
  
```127.0.0.1:6379> HVALS userInfo2
1) "jankz"
2) "jz"
3) "123456"
4) "jankz@jankz.com"
```
###HEXISTS 检测hash中key的field是否存在
HEXISTS userInfo2 username
HEXISTS userInfo2 notExists
###HLEN 返回hash 表中key 中 field的数量
语法: HLEN key
HLEN userInfo2
###HINCRBY 给hash 中的key的field做增量操作
语法 HINCRBY KEY FIELD increment
HSET userInfo3 age 12
HINCRBY userInfo3 age 10

```127.0.0.1:6379> HSET userInfo3 age 12
(integer) 1
127.0.0.1:6379> HINCRBY userInfo3 age 10
(integer) 22 
```
###HINCRBYFLOAT 加浮点数

语法 HINCRBYFLOAR KEY FIELD increment

HSET userInfo3 salary 1200
HINCRBYFLOAT userInfo3 salary 10.11

```127.0.0.1:6379> HSET userInfo3 salary 1200
(integer) 1
127.0.0.1:6379> HINCRBYFLO
```

###HDEL :删除hash中key的指定域
语法 :HDEL KEY FIELD FIELD 
注意: 
HDEL userInfo2 username password

##LIST类型
双向
###LPUSG 向列表左端添加元素
语法: LPUSH key value value ...
LPUSH testList1 a b c

```127.0.0.1:6379[1]> EXISTS testList1
(integer) 0
127.0.0.1:6379[1]> LPUSH testList1 a b c
(integer) 3
127.0.0.1:6379[1]> LPUSH testList1 c d e
(integer) 6
127.0.0.1:6379[1]> LRANGE testList1
(error) ERR wrong number of arguments for 'lrange' command
127.0.0.1:6379[1]> LRANGE testList1 0 -1
1) "e"
2) "d"
3) "c"
4) "c"
5) "b"
6) "a"
```
### RPUSH 向列表右端添加元素
 RPUSH testList1 test1 test2
```127.0.0.1:6379[1]> RPUSH testList1 test1 test2
(integer) 9
127.0.0.1:6379[1]> LRANGE testList1 0 -1
1) "j"
2) "e"
3) "d"
4) "c"
5) "c"
6) "b"
7) "a"
8) "test1"
9) "test2"
```
###LPUSHX :向列表头部添加元素,只有key存在才添加
LPUSHX myList1 A

```127.0.0.1:6379[1]> LPUSHX myList A B C
(error) ERR wrong number of arguments for 'lpushx' command
```
###RPUSHX:向列表尾部添加元素,只有key存在才添加
语法: RPUSHX key value

###LPOP :将列表头部元素弹出

```127.0.0.1:6379[1]> LRANGE testList1 0 -1
 1) "j"
 2) "e"
 3) "d"
 4) "c"
 5) "c"
 6) "b"
 7) "a"
 8) "test1"
 9) "test2"
10) "test4"
11) "test3"
127.0.0.1:6379[1]> LPOP testList1
"j"
127.0.0.1:6379[1]> RPOP testList1
"test3"
127.0.0.1:6379[1]> RPOP testList11
(nil)
127.0.0.1:6379[1]> LPOP testList11
(nil)
```
###LLEN 获得列表长度
语法: LLEN key
LLEN testList1
```127.0.0.1:6379[1]> LLEN testList1
(integer) 9
127.0.0.1:6379[1]> LLEN testList11
(integer) 0
```
###LRANGE :获得列表片段
语法:LRANGE key start stop
LRANGE testList1 0 -1 从第一个到最后一个
```127.0.0.1:6379[1]> LRANGE testList1 0 -1
1) "e"
2) "d"
3) "c"
4) "c"
5) "b"
6) "a"
7) "test1"
8) "test2"
9) "test4"
127.0.0.1:6379[1]> LRANGE testList1 0 0
1) "e"
127.0.0.1:6379[1]> LRANGE testList1 0 2
1) "e"
2) "d"
3) "c"
127.0.0.1:6379[1]> LRANGE testList1 -3 -1
1) "test1"
2) "test2"
3) "test4"
127.0.0.1:6379[1]> LRANGE testList1 100 3
(empty list or set)
127.0.0.1:6379[1]> LRANGE testList1 0 300
1) "e"
2) "d"
3) "c"
4) "c"
5) "b"
6) "a"
7) "test1"
8) "test2"
9) "test4"
127.0.0.1:6379[1]> 
```

###LREM :删除列表中指定的值
语法: LREM key count value
count值有以下几种
count>0 从开头向尾搜索 移除与value相等的元素 count 个
count<0 从尾向头部搜索 移除与value相等的元素 count 个
count= 0 移除列表中所有与value相等的值
LPUSH myTest2 a b c d a b v d c s

```127.0.0.1:6379[1]> LPUSH myTest2 a b c d a b v d c s
(integer) 10
127.0.0.1:6379[1]> LRANGE myTest2 0 -1
 1) "s"
 2) "c"
 3) "d"
 4) "v"
 5) "b"
 6) "a"
 7) "d"
 8) "c"
 9) "b"
10) "a"
127.0.0.1:6379[1]> LLEN myTest2
(integer) 10
127.0.0.1:6379[1]> LREM myTest2 2 b
(integer) 2
127.0.0.1:6379[1]> LRANGE myTest2 0 -1
1) "s"
2) "c"
3) "d"
4) "v"
5) "a"
6) "d"
7) "c"
8) "a"
127.0.0.1:6379[1]> LLEN myTest2
(integer) 8
127.0.0.1:6379[1]> LREM myTest2 -2 c
(integer) 2
127.0.0.1:6379[1]> LRANGE myTest2 0 -1
1) "s"
2) "d"
3) "v"
4) "a"
5) "d"
6) "a"
127.0.0.1:6379[1]> LREM myTest2 0 a
(integer) 2
127.0.0.1:6379[1]> LRANGE myTest2 0 -1
1) "s"
2) "d"
3) "v"
4) "d"
127.0.0.1:6379[1]> 
```
###LINDEX:获得指定索引元素的值
语法:LINDEX key value

```127.0.0.1:6379[1]> LINDEX myTest2 4
"j"
127.0.0.1:6379[1]> LINDEX myTest2 5
"k"
127.0.0.1:6379[1]> LINDEX myTest2 9
"a"
127.0.0.1:6379[1]> LINDEX myTest2 10
"a"
127.0.0.1:6379[1]> LINDEX myTest2 11
"s"
127.0.0.1:6379[1]> LINDEX myTest2 12
(nil)
```

###LSET:设定指定索引元素的值
语法:LSET key index value

```127.0.0.1:6379[1]> LINDEX myTest2 11
"s"
127.0.0.1:6379[1]> LINDEX myTest2 12
(nil)
127.0.0.1:6379[1]> LSET myTest2 11 ss
OK
127.0.0.1:6379[1]> LINDEX myTest2 11
"ss"
```
###LTRIM 只保留列表片段
语法: LTRIM key start stop 
LPUSH myList3 log1 log2 log3 log4 log5
LTRIM myList2 0 2

```127.0.0.1:6379[1]> LPUSH myList3 log1 log2 log3 log4 log5
(integer) 5
127.0.0.1:6379[1]> LRANGE myList3 0 -1
1) "log5"
2) "log4"
3) "log3"
4) "log2"
5) "log1"
127.0.0.1:6379[1]> LTRIM myList3 0 2
OK
127.0.0.1:6379[1]> LRANGE myList3 0 -1
1) "log5"
2) "log4"
3) "log3"
127.0.0.1:6379[1]> LPUSH myList3 log11 log12 log13 log14 log15
(integer) 8
127.0.0.1:6379[1]> LTRIM myList3 0 1
OK
127.0.0.1:6379[1]> LRANGE myList3 0 -1
1) "log15"
2) "log14"
127.0.0.1:6379[1]> LTRIM myList3 100 300
OK
127.0.0.1:6379[1]> LRANGE myList3 0 -1
(empty list or set)
127.0.0.1:6379[1]> 
```
###LINSERT :项列表插入元素
语法:LINSERT key BEFORE|AFTER pivot value
LPUSH myList5 a b c d
LINSERT myList5 BEFORE 'b' 'Jz'
LINSERT myList5 AFTER 'd' 'jzopen'
```127.0.0.1:6379[1]> LPUSH myList5 a b c d
(integer) 4
127.0.0.1:6379[1]> LINSERT myList5 BEFORE 'b' 'Jz'
(integer) 5
127.0.0.1:6379[1]> LRANGE myList5 0 -1
1) "d"
2) "c"
3) "Jz"
4) "b"
5) "a"
127.0.0.1:6379[1]> LINSERT myList5 BEFORE 'bb' 'Jz'
(integer) -1 //表示没有
127.0.0.1:6379[1]> LINSERT myList5 AFTER 'd' 'jzopen'
(integer) 6
127.0.0.1:6379[1]> LRANGE myList5 0 -1
1) "d"
2) "jzopen"
3) "c"
4) "Jz"
5) "b"
6) "a"
```
###RPOPLPUSH:将元素从一个列表转到另一个列表
语法:RPOPLPUSH source destination
LPUSH myList6 a b c
LPUSH myList7 x y z
RPOPLPUSH myList6 myList7

LPUSH weblog log1 log2 log3 log4
RPOPLPUSH weblog weblog
```127.0.0.1:6379[1]> LPUSH myList6 a b c 
(integer) 3
127.0.0.1:6379[1]> LPUSH myList7 x y z 
(integer) 3
127.0.0.1:6379[1]> RPOPLPUSH myList6 myList7
"a"
127.0.0.1:6379[1]> LRANGE myList7 0 -1
1) "a"
2) "z"
3) "y"
4) "x"

```

```127.0.0.1:6379[1]> LPUSH weblog log1 log2 log3 log4
(integer) 4
127.0.0.1:6379[1]> RPOPLPUSH weblog weblog
"log1"
127.0.0.1:6379[1]> RPOPLPUSH weblog weblog
"log2"
127.0.0.1:6379[1]> RPOPLPUSH weblog weblog
"log3"
127.0.0.1:6379[1]> RPOPLPUSH weblog weblog
"log4"
127.0.0.1:6379[1]> RPOPLPUSH weblog weblog
"log1"
127.0.0.1:6379[1]> RPOPLPUSH weblog weblog
"log2"
127.0.0.1:6379[1]> RPOPLPUSH weblog weblog
"log3"
127.0.0.1:6379[1]> RPOPLPUSH weblog weblog
"log4"
127.0.0.1:6379[1]> RPOPLPUSH weblog weblog
"log1"
127.0.0.1:6379[1]> RPOPLPUSH weblog1 weblog
(nil)
```
###BLPOP : LPOP的阻塞版本
语法:BLPOP key [key...] timeout
LPUSH myList9 a b c
LPUSH myList10 d e f
BLPOP myList8 myList9 myList10 0

```127.0.0.1:6379[1]> LPUSH myList9 a b c
(integer) 3
127.0.0.1:6379[1]> LPUSH myList10 d e f
(integer) 3
127.0.0.1:6379[1]> BLPOP myList8 myList9 myList10 0
1) "myList9"
2) "c"
127.0.
```

```127.0.0.1:6379[1]> LPUSH myList8 a b c
(integer) 3
127.0.0.1:6379[1]>
```
```127.0.0.1:6379[1]> BLPOP myList8 0
1) "myList8"
2) "c"
(63.54s)
127.0.0.1:6379[1]> 
```
支持多个客户端同时操作 先阻塞先操作
##SET 类型 无序集合
###SADD :向集合中添加元素
语法:SADD key number[,...]
SADD web jzopen.com
SADD web taobao.com jd.com janzk.com
SADD web taobao.com jankz.com //会把不重复的添加进去

SMEMBERS web
```127.0.0.1:6379[1]> SADD web jzopen.com
(integer) 1
127.0.0.1:6379[1]> SADD web jzopen.com
(integer) 0
127.0.0.1:6379[1]> SADD web taobao.com jd.com janzk.com
(integer) 3
127.0.0.1:6379[1]> SMEMBERS
(error) ERR wrong number of arguments for 'smembers' command
127.0.0.1:6379[1]> SMEMBERS web
1) "taobao.com"
2) "janzk.com"
3) "jzopen.com"
4) "jd.com"
127.0.0.1:6379[1]> SMEMBERS web123
(empty list or set)
```
###SMEMBERS: 返回指定集合中的元素
语法:SMEMBERS key  
###SISMEMBER :检测value是否是集合成员
语法:SISMEMBER key member
SISMEMBER web jankz.com

```127.0.0.1:6379[1]> SMEMBERS web123
(empty list or set)
127.0.0.1:6379[1]>  SISMEMBER web jankz.com
(integer) 0
127.0.0.1:6379[1]>  SISMEMBER web janzk.com
(integer) 1
```
###SREM 删除集合中一个或者几个成员
语法:SREM key member []
SREM web jd.com

```127.0.0.1:6379[1]> SREM web jd.com
(integer) 1
127.0.0.1:6379[1]> SMEMBERS web
1) "taobao.com"
2) "janzk.com"
3) "jzopen.com"
127.0.0.1:6379[1]> SADD web jankz.com
(integer) 1
127.0.0.1:6379[1]> SMEMBERS web
1) "taobao.com"
2) "janzk.com"
3) "jzopen.com"
4) "jankz.com"
127.0.0.1:6379[1]> SREM web jd.com
(integer) 0
127.0.0.1:6379[1]> SREM web taobao.com jankz.com
(integer) 2
```
###SPOP 随机删除一个元素 并返回
SPOP key

```127.0.0.1:6379[1]> SADD course java php ajax html css xss 
(integer) 6
127.0.0.1:6379[1]> SPOP course
"html"
127.0.0.1:6379[1]> SPOP course
"java"
127.0.0.1:6379[1]> SPOP course
"css"
```
###SRANDMEMBER :随机返回集合中的元素
SRANDMEMBER course
```.
127.0.0.1:6379[1]> SRANDMEMBER course 
"ajax"
127.0.0.1:6379[1]> SRANDMEMBER course 
"php"
127.0.0.1:6379[1]> SRANDMEMBER course 
"php"
127.0.0.1:6379[1]> SRANDMEMBER course 
"xss"
```
SRANDMEMBER course 2

```127.0.0.1:6379[1]> SRANDMEMBER course 2
1) "xss"
2) "php"
127.0.0.1:6379[1]> SRANDMEMBER course 10
1) "ajax"
2) "xss"
3) "php"
127.0.0.1:6379[1]> SRANDMEMBER course 0
(empty list or set)
127.0.0.1:6379[1]> SRANDMEMBER course -2
1) "ajax"
2) "ajax"
127.0.0.1:6379[1]> SRANDMEMBER course -3
1) "ajax"
2) "ajax"
3) "ajax"
127.0.0.1:6379[1]> SRANDMEMBER course -2
1) "xss"
```
注意:count为正数 而且小于集合的元素,返回的一个包含随机元素的集合数组
count 数大于集合元素个数 这个时候返回整个集合
count为负数 返回一个数组 数组的成员可能重复出现,数组的长度是count取绝对值.
 
###SDIFF 返回集合差集
语法:SDIFF key1 key2
SADD testSet1 a b c d 
SADD testSet2 b c d e

```127.0.0.1:6379[1]> SADD testSet1 a b c d
(integer) 4
127.0.0.1:6379[1]> SADD testSet2 b c d e
(integer) 4
127.0.0.1:6379[1]> SDIFF testSet1 testSet2
1) "a"
127.0.0.1:6379[1]> SDIFF testSet2 testSet1
1) "e"
127.0.0.1:6379[1]> SADD testSet3 x y z
(integer) 3
127.0.0.1:6379[1]> SDIFF testSet2 testSet1 testSet3
1) "e"
```
###SINTER :返回集合交集
SINTER testSet1 testSet2

```127.0.0.1:6379[1]> SINTER testSet1 testSet2
1) "d"
2) "b"
3) "c"
```
###SUNION:返回并集
SUNION testSet1 testSet2

```127.0.0.1:6379[1]> SUNION testSet1 testSet2
1) "e"
2) "d"
3) "b"
4) "c"
5) "a"
```
###SCARD 获得集合长度

```127.0.0.1:6379[1]> SCARD testSet1
(integer) 4
```
###SDIFFSTORE
语法:SDIFFSTORE destination key key ...
SDIFFSTORE diffset testSet1 testSet2

SDIFFSTORE diffset testSet1 testSet1 testSet2

```127.0.0.1:6379[1]> SMEMBERS diffset
(empty list or set)
127.0.0.1:6379[1]> SMEMBERS testSet1
1) "d"
2) "b"
3) "c"
4) "a"
127.0.0.1:6379[1]> SMEMBERS testSet2
1) "e"
2) "d"
3) "b"
4) "c"
127.0.0.1:6379[1]> SADD testSet2 x y z
(integer) 3
127.0.0.1:6379[1]> SMEMBERS testSet2
1) "c"
2) "e"
3) "x"
4) "z"
5) "b"
6) "d"
7) "y"
127.0.0.1:6379[1]> SDIFFSTORE diffset testSet1 testSet1 testSet2
(integer) 0
127.0.0.1:6379[1]> SDIFFSTORE diffset testSet1  testSet2
(integer) 1
```

###SUNIONSTORE :将并集的结果存在指定集合
语法:SUNIONSTORE destination key key ...

```127.0.0.1:6379[1]> SDIFFSTORE diffset testSet1 testSet1 testSet2
(integer) 0
127.0.0.1:6379[1]> SDIFFSTORE diffset testSet1  testSet2
(integer) 1
127.0.0.1:6379[1]> SUNIONSTORE unionset testSet1  testSet2
(integer) 8
127.0.0.1:6379[1]> SMEMBERS unionset
1) "x"
2) "e"
3) "z"
4) "d"
5) "b"
6) "y"
7) "c"
8) "a"
```
###SINTERSTORE :交集的结果存在指定集合
语法:SINTERSTORE destination key key ...
```127.0.0.1:6379[1]> SINTERSTORE inserset testSet1  testSet2
(integer) 3
127.0.0.1:6379[1]> SMEMBERS inserset
1) "b"
2) "d"
3) "c"
```
###SMOVE 将集合中元素移动另一个集合中
语法: SMOVE source destination member


```127.0.0.1:6379[1]> SMEMBERS testSet2
1) "e"
2) "x"
3) "z"
4) "b"
5) "d"
6) "y"
7) "c"
127.0.0.1:6379[1]> SMOVE testSet2 testStr1 x
(integer) 1
127.0.0.1:6379[1]> SMEMBERS testSet1
1) "d"
2) "b"
3) "c"
4) "a"
127.0.0.1:6379[1]> SMEMBERS testSet2
1) "e"
2) "z"
3) "b"
4) "d"
5) "y"
6) "c"
127.0.0.1:6379[1]> SMOVE testSet2 testSet1 y
(integer) 1
127.0.0.1:6379[1]> SMEMBERS testSet1
1) "d"
2) "b"
3) "c"
4) "a"
5) "y"
127.0.0.1:6379[1]> SMOVE testSet2 testSet1 y
(integer) 0
127.0.0.1:6379[1]> SADD testStr2 y
(integer) 1
127.0.0.1:6379[1]> SMOVE testSet2 testSet1 y
(integer) 0

```
##有序集合 ZSET(sorted set)
ZSET

###ZADD 将元素及其分数添加到集合
语法:ZADD key score member [score member]
ZADD phpScore 100 king 
ZADD phpScore 99 maizi 80 jankz 20 test1 80 test3
//返回新添加的数量
ZADD phpScore +inf maxInt -inf minInt
正无穷 负无穷

```127.0.0.1:6379[1]> ZADD phpScore +inf maxInt -inf minInt
(integer) 2
127.0.0.1:6379[1]> ZSCORE phpScore minInt
"-inf"
127.0.0.1:6379[1]> ZSCORE phpScore maxInf
(nil)
127.0.0.1:6379[1]> ZSCORE phpScore maxInt
"inf"
```
###ZSCORE :获得指定元素的分数
语法:phpScore 

```(integer) 0
127.0.0.1:6379[1]> ZADD phpScore 100 king 
(integer) 1
127.0.0.1:6379[1]> ZADD phpScore 99 maizi 80 jankz 20 test1 80 test3
(integer) 4
127.0.0.1:6379[1]> ZSCORE phpScore jankz
"80"
```
###ZRANGE :按照元素分数从小到大的顺序返回指定索引start 到stop 之间所有的元素.包含两端
ZRANGE phpScore 0 -1
```127.0.0.1:6379[1]> ZRANGE phpScore 0 -1
1) "minInt"
2) "test1"
3) "jankz"
4) "test3"
5) "maizi"
6) "king"
7) "test4"
8) "maxInt"
```
从小到大
ZRANGE phpScore 0 -1 WITHSCORES
ZRANGE phpScore 0 2 WITHSCORES
ZRANGE phpScore 0 100 WITHSCORES

```127.0.0.1:6379[1]> ZRANGE phpScore 0 -1 WITHSCORES
 1) "minInt"
 2) "-inf"
 3) "test1"
 4) "20"
 5) "jankz"
 6) "80"
 7) "test3"
 8) "80"
 9) "maizi"
10) "99"
11) "king"
12) "100"
13) "test4"
14) "102.5"
15) "maxInt"
16) "inf
127.0.0.1:6379[1]> ZRANGE phpScore 0 2 WITHSCORES
1) "minInt"
2) "-inf"
3) "test1"
4) "20"
5) "jankz"
6) "80"
127.0.0.1:6379[1]> ZRANGE phpScore 0 100 WITHSCORES
 1) "minInt"
 2) "-inf"
 3) "test1"
 4) "20"
 5) "jankz"
 6) "80"
 7) "test3"
 8) "80"
 9) "maizi"
10) "99"
11) "king"
12) "100"
13) "test4"
14) "102.5"
15) "maxI
```
###
ZADD phpScore 60 test6 60 test7 60 test8 60 test9
当两个元素的分数相同的时候 Redis在排序按照字典的顺序(0<9 A<Z<a<z)
如果使用的是UTF-8的编码方式的中文 同样按照字典顺序排列

###ZREVRANGE :按照分数从大到小饿顺序
语法:ZREVRANGE key start stop
ZREVRANGE phpScore 0 -1 WITHSCORES

```127.0.0.1:6379[1]> ZREVRANGE phpScore 0 -1 WITHSCORES
 1) "maxInt"
 2) "inf"
 3) "test4"
 4) "102.5"
 5) "king"
 6) "100"
 7) "maizi"
 8) "99"
 9) "test3"
10) "80"
11) "jankz"
12) "80"
13) "test9"
14) "60"
15) "test8"
16) "60"
17) "test7"
18) "60"
19) "test6"
20) "60"
21) "test1"
22) "20"
23) "minInt"
24) "-inf
```
### ZRANGEBYSCORE : 获得指定分数范围内的元素 按照分数从小到大 返回是分数在指定的min到max的元素
语法:ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT OFFSET COUNT]
//获得分数80-90之间的
ZRANGEBYSCORE phpScore 80 90

```27.0.0.1:6379[1]> ZRANGEBYSCORE phpScore 80 90
1) "jankz"
2) "test3"
127.0.0.1:6379[1]> ZADD phpScore 81 test9 82 test10 88 test11 98 test12
(integer) 3
127.0.0.1:6379[1]> ZRANGEBYSCORE phpScore 80 90
1) "jankz"
2) "test3"
3) "test9"
4) "test10"
5) "test11"
```
ZRANGEBYSCORE phpScore 80 (90 WITHSCORES
括号表示不包括90
ZRANGEBYSCORE phpScore (80 (90 WITHSCORES
注意:通过 ( 不包含端点
```127.0.0.1:6379[1]> ZADD phpScore 90 test13
(integer) 1
127.0.0.1:6379[1]> ZRANGEBYSCORE phpScore 80 (90 WITHSCORES)
(error) ERR syntax error
127.0.0.1:6379[1]> ZRANGEBYSCORE phpScore 80 (90 WITHSCORES
 1) "jankz"
 2) "80"
 3) "test3"
 4) "80"
 5) "test9"
 6) "81"
 7) "test10"
 8) "82"
 9) "test11"
10) "88"

127.0.0.1:6379[1]> ZRANGEBYSCORE phpScore (80 (90 WITHSCORES
1) "test9"
2) "81"
3) "test10"
4) "82"
5) "test11"
6) "88"
```
ZRANGEBYSCORE phpScore 90 +inf WITHSCORES 

```127.0.0.1:6379[1]> ZRANGEBYSCORE phpScore 90 +inf WITHSCORES 
 1) "test13"
 2) "90"
 3) "test12"
 4) "98"
 5) "maizi"
 6) "99"
 7) "king"
 8) "100"
 9) "test4"
10) "102.5"
11) "maxInt"
12) "inf"
```
ZRANGEBYSCORE phpScore 90 +inf WITHSCORES LIMIT 0 3
偏移量0 显示条数 3
```127.0.0.1:6379[1]> ZRANGEBYSCORE phpScore 90 +inf WITHSCORES LIMIT 0 3
1) "test13"
2) "90"
3) "test12"
4) "98"
5) "maizi"
6) "99"
```
ZRANGEBYSCORE phpScore 80 +inf WITHSCORES LIMIT 3 3

```127.0.0.1:6379[1]> ZRANGEBYSCORE phpScore 80 +inf WITHSCORES LIMIT 3 3
1) "test10"
2) "82"
3) "test11"
4) "88"
5) "test13"
6) "90"
```

###ZREVRANGEBYSCORE :获得指定范围内的元素 按照元素的分数从大到小的顺序返回min 和 max之间的元素
语法:ZREVRANGEBYSCORE key max min [WITHSCORES] [LIMIT OFFSET COUNT]
ZREVRANGEBYSCORE phpScore 90 80 WITHSCORES

```127.0.0.1:6379[1]> ZREVRANGEBYSCORE phpScore 90 80 WITHSCORES
 1) "test13"
 2) "90"
 3) "test11"
 4) "88"
 5) "test10"
 6) "82"
 7) "test9"
 8) "81"
 9) "test3"
10) "80"
11) "jankz"
12) "80"
```
ZREVRANGEBYSCORE phpScore 90 80 WITHSCORES LIMIT 0 3

```127.0.0.1:6379[1]> ZREVRANGEBYSCORE phpScore 90 80 WITHSCORES LIMIT 0 3
1) "test13"
2) "90"
3) "test11"
4) "88"
5) "test10"
6) "82"
```
###ZINCRBY :操作某个元素的分数 返回操作之后的分数
语法:ZINCRBY key increment member
ZINCRBY phpScore 10 test13
ZINCRBY phpScore -21 test13

```127.0.0.1:6379[1]> ZINCRBY phpScore 10 test13
"100"
127.0.0.1:6379[1]> ZINCRBY phpScore -21 test13
"79"
```
###ZCARD :获得集合元素的数量
ZCARD key

```127.0.0.1:6379[1]> ZCARD phpScore
(integer) 16
```
###ZCOUNT 获得指定分数内的元素个数
ZCOUNT key min max
ZCOUNT phpScore (80 (90

```127.0.0.1:6379[1]> ZCOUNT phpScore (80 (90
(integer) 3
```
###ZREM 删除一个或者多个元素 返回删除元素的个数
ZREM key member ...
ZREM phpScore test9 test10

```127.0.0.1:6379[1]> ZREM phpScore test9 test10
(integer) 2
127.0.0.1:6379[1]> ZCARD phpScore
(integer) 14
```
###ZREMRANGEBYRANK :按照排名范围删除元素 按照从小到大的顺序删除指定的排名内的所有元素
语法:ZREMRANGEBYRANK key start stop

ZADD testRem 1 a 2 b 3 c 4 d 5 e 6 f 7 g 8 h
```127.0.0.1:6379[1]> ZADD testRem 1 a 2 b 3 c 4 d 5 e 6 f 7 g 8 h
(integer) 8
127.0.0.1:6379[1]> ZCOUNT testRem
(error) ERR wrong number of arguments for 'zcount' command
127.0.0.1:6379[1]> ZCARD testRem
(integer) 8
127.0.0.1:6379[1]> ZRANGE testRem 0 -1 WITHSCORES
 1) "a"
 2) "1"
 3) "b"
 4) "2"
 5) "c"
 6) "3"
 7) "d"
 8) "4"
 9) "e"
10) "5"
11) "f"
12) "6"
13) "g"
14) "7"
15) "h"
16) "8
```
删除前三个最小的
ZREMRANGEBYRANK testRem 0 2

```127.0.0.1:6379[1]> ZREMRANGEBYRANK testRem 0 2
(integer) 3
127.0.0.1:6379[1]> ZRANGE testRem 0 -1 WITHSCORES
 1) "d"
 2) "4"
 3) "e"
 4) "5"
 5) "f"
 6) "6"
 7) "g"
 8) "7"
 9) "h"
10) "8"
```
###ZREMRANGEBYSCORE :按照分数范围删除元素
ZADD testRem1 1 a 2 b 3 c 4 d 5 e 6 f 7 g 8 h
ZREMRANGEBYSCORE testRem1 3 7

```127.0.0.1:6379[1]> ZADD testRem1 1 a 2 b 3 c 4 d 5 e 6 f 7 g 8 h
(integer) 8
127.0.0.1:6379[1]> ZREMRANGEBYSCORE testRem1 3 7
(integer) 5
127.0.0.1:6379[1]> ZRANGE testRem1 0 -1 WITHSCORES
1) "a"
2) "1"
3) "b"
4) "2"
5) "h"
6) "8"
127.0.0.1:6379[1]> 
```
###ZRANK 获得某个元素的排名 根据分数从小到大
语法:ZRANK key 
ZRANK phpScore king

```127.0.0.1:6379[1]> ZRANGE phpScore 0 -1
 1) "minInt"
 2) "test1"
 3) "test6"
 4) "test7"
 5) "test8"
 6) "test13"
 7) "jankz"
 8) "test3"
 9) "test11"
10) "test12"
11) "maizi"
12) "king"
13) "test4"
14) "maxInt"
127.0.0.1:6379[1]> ZRANK phpScore king
(integer) 11
```
###ZREVRANK 获得某个元素的排名 根据分数从大到小

语法:ZREVRANK key 
ZREVRANK phpScore king

```6) "8"
127.0.0.1:6379[1]> ZRANGE phpScore 0 -1
 1) "minInt"
 2) "test1"
 3) "test6"
 4) "test7"
 5) "test8"
 6) "test13"
 7) "jankz"
 8) "test3"
 9) "test11"
10) "test12"
11) "maizi"
12) "king"
13) "test4"
14) "maxInt"
127.0.0.1:6379[1]> ZRANK phpScore king
(integer) 11
127.0.0.1:6379[1]> ZREVRANK phpScore king
(integer) 2
```
###ZINTERSCORE :计算有序集合的交集 并经结果存储
语法:ZINTERSTORE destination numkeys [WEIGHTS weight weight...] [AGGREGATE SUM|MIN|MAX]

ZADD testSorted1 1 a 2 b 3 c

ZADD testSorted2 10 a 20 b 30 c

ZINTERSTORE resTestSorted1 2 testSorted1 testSorted2

```127.0.0.1:6379[1]> ZADD testSorted1 1 a 2 b 3 c
(integer) 3
127.0.0.1:6379[1]> ZADD testSorted2 10 a 20 b 30 c
(integer) 3

127.0.0.1:6379[1]> ZINTERSTORE resTestSorted1 2 testSorted1 testSorted2
(integer) 3
127.0.0.1:6379[1]> ZRANGE resTestSorted1 0 -1
1) "a"
2) "b"
3) "c"
127.0.0.1:6379[1]> ZRANGE resTestSorted1 0 -1 WITHSCORES
1) "a"
2) "11"
3) "b"
4) "22"
5) "c"
6) "33"
```
ZINTERSTORE resTestSorted1 2 testSorted1 testSorted2 AGGREGATE SUM

```127.0.0.1:6379[1]> ZINTERSTORE resTestSorted1 2 testSorted1 testSorted2 AGGREGATE SUM
(integer) 3
127.0.0.1:6379[1]> ZRANGE resTestSorted1 0 -1 WITHSCORES
1) "a"
2) "11"
3) "b"
4) "22"
5) "c"
6) "33"

```
ZINTERSTORE resTestSorted2 2 testSorted1 testSorted2 AGGREGATE MIN
```127.0.0.1:6379[1]> ZINTERSTORE resTestSorted2 2 testSorted1 testSorted2 AGGREGATE MIN
(integer) 3
127.0.0.1:6379[1]> ZRANGE resTestSorted2 0 -1 WITHSCORES
1) "a"
2) "1"
3) "b"
4) "2"
5) "c"
6) "3"
```
ZINTERSTORE resTestSorted3 2 testSorted1 testSorted2 AGGREGATE MAX

```127.0.0.1:6379[1]> ZINTERSTORE resTestSorted3 2 testSorted1 testSorted2 AGGREGATE MAX
(integer) 3
127.0.0.1:6379[1]> ZRANGE resTestSorted3 0 -1 WITHSCORES
1) "a"
2) "10"
3) "b"
4) "20"
5) "c"
6) "30"
127.0.0.1:6379[1]> 
```
###ZINTERSTORE resTestSorted5 2 testSorted1 testSorted2 WEIGHTS 2 0.2

```127.0.0.1:6379[1]> ZINTERSTORE resTestSorted5 2 testSorted1 testSorted2 WEIGHTS 2 0.2
(integer) 3
127.0.0.1:6379[1]> ZRANGE resTestSorted5 0 -1 WITHSCORES
1) "a"
2) "4"
3) "b"
4) "8"
5) "c"
6) "12"
```
###ZUNIONSTORE :计算有序集合并集 将结果保存起来
语法:ZUNIONSTORE destination numkeys [WEIGHTS weight weight...] [AGGREGATE SUM|MIN|MAX]
ZADD testUnion1 1 a 2 b 3 c
ZADD testUnion2 4 d 5 e 6 f 7 a
ZUNIONSTORE uniRes1 2 testUnion1 testUnion2

```127.0.0.1:6379[1]> ZADD testUnion1 1 a 2 b 3 c
(integer) 3
127.0.0.1:6379[1]> ZADD testUnion2 4 d 5 e 6 f 7 a
(integer) 4
127.0.0.1:6379[1]> ZUNIONSTORE uniRes1 2 testUnion1 testUnion2
(integer) 6
127.0.0.1:6379[1]> ZRANGE uniRes1 0 -1
1) "b"
2) "c"
3) "d"
4) "e"
5) "f"
6) "a"
127.0.0.1:6379[1]> ZRANGE uniRes1 0 -1 WITHSCORES
 1) "b"
 2) "2"
 3) "c"
 4) "3"
 5) "d"
 6) "4"
 7) "e"
 8) "5"
 9) "f"
10) "6"
11) "a"
12) "8"
```
ZUNIONSTORE uniRes2 2 testUnion1 testUnion2 AGGREGATE MIN

```127.0.0.1:6379[1]> ZUNIONSTORE uniRes2 2 testUnion1 testUnion2 AGGREGATE MIN
(integer) 6
127.0.0.1:6379[1]> ZRANGE uniRes2 0 -1 WITHSCORES
 1) "a"
 2) "1"
 3) "b"
 4) "2"
 5) "c"
 6) "3"
 7) "d"
 8) "4"
 9) "e"
10) "5"
11) "f"
12) "6"
```
ZUNIONSTORE uniRes3 2 testUnion1 testUnion2 AGGREGATE MAX

```127.0.0.1:6379[1]> ZUNIONSTORE uniRes3 2 testUnion1 testUnion2 AGGREGATE MAX
(integer) 6
127.0.0.1:6379[1]> ZRANGE uniRes3 0 -1 WITHSCORES
 1) "b"
 2) "2"
 3) "c"
 4) "3"
 5) "d"
 6) "4"
 7) "e"
 8) "5"
 9) "f"
10) "6"
11) "a"
12) "7"
```
ZUNIONSTORE uniRes4 2 testUnion1 testUnion2 WEIGHTS 2 2
WEIGHTS 2 2 权重
```127.0.0.1:6379[1]> ZUNIONSTORE uniRes4 2 testUnion1 testUnion2 WEIGHTS 2 2
(integer) 6
127.0.0.1:6379[1]> ZRANGE uniRes4 0 -1 WITHSCORES
 1) "b"
 2) "4"
 3) "c"
 4) "6"
 5) "d"
 6) "8"
 7) "e"
 8) "10"
 9) "f"
10) "12"
11) "a"
12) "16"
```


#和key相关的命令
##KEYS 返回所有符合给定模式的key
* : 匹配人一个字符
? :匹配任意一个字符
[]:匹配[]之间的一个字符,[a-z]
\x:匹配特殊字符\?\*;
keys *
keys *o*
keys t??
keys ?o*
keys a[a-z]*
##EXISTS 检测指定key是否存在
语法:EXISTS key
EXISTS one
###TYPE :返回key所存储的类型
TYPE userInfo2

```127.0.0.1:6379> TYPE userInfo2
hash
```
不存在返回none
string
hash
list
set 
zset
##EXPIRE 设置key的过期时间
语法: EXPIRE key seconds
SET cache_page 'http://jzopen.com'
EXPIRE cache_page 100


```127.0.0.1:6379> TYPE userInfo2
hash
127.0.0.1:6379> SET cache_page 'http://jzopen.com'
OK
127.0.0.1:6379> EXPIRE cache_page 100
(integer) 1
127.0.0.1:6379> TTL cache_page
(integer) 91
127.0.0.1:6379> TTL cache_page
(integer) 90
127.0.0.1:6379> TTL cache_page
(integer) 89
127.0.0.1:6379> TTL cache_page
(integer) 88
127.0.0.1:6379> TTL cache_page
(integer) 85
127.0.0.1:6379>  
```
##EXPIREAT 在指定时间戳过期
SET cache_page2 'http://jzopen.com'
EXPIREAT cache_page2 15000000(时间戳)

##PEXPIRE 以毫秒形式指定过期时间
PEXPIRE key milliseconds
PEXPIRE cache_pag2 50000
##PEXPIREAT 指定时间戳 单位毫秒
语法:PEXPIREAT KEY timestamp
PEXPIREAT cache_page2  1430000000000
##TTL以秒为单位返回key的剩余时间
##PTTL 以毫秒返回key的剩余时间
如果key没有设置剩余时间 返回-1 如果key不存在 返回-2

##PERSIST:将一个带有过期时间的key变为永久
语法:PERSIST key 
SET cache 'testCache'
EXPIRE cache 100
TTL cache
PERSIST cache
TTL cache

```127.0.0.1:6379> SET cache 'testCache'
OK
127.0.0.1:6379> EXPIRE cache 100
(integer) 1
127.0.0.1:6379> TTL cache
(integer) 94
127.0.0.1:6379> TTL cache
(integer) 94
127.0.0.1:6379> PERSIST cache
(integer) 1
127.0.0.1:6379> TTL cache
(integer) -1
127.0.0.1:6379> PERSIST cache
(integer) 0
```
##删除指定key
DEL key
```.
127.0.0.1:6379> DEL cache
(integer) 1
```
##RANDOMKEY 随机的从当前数据库中返回一个key
语法:RANDOMKEY

```127.0.0.1:6379> RANDOMKEY
"testStr11"
127.0.0.1:6379> RANDOMKEY
"testStr"
127.0.0.1:6379> RANDOMKEY
"testStr"
127.0.0.1:6379> RANDOMKEY
"counter2"
127.0.0.1:6379> RANDOMKEY
```
##RENAME 重命名一个key
SET testRename1 'rename1'
RENAME testRename1 testRename2

RENAME testRename2 testRename2
RENAME testRename2 testStr11

```127.0.0.1:6379> SET testRename1 'rename1'
OK
127.0.0.1:6379> RENAME testRename1 testRename2
OK
127.0.0.1:6379> RENAME testRename2 testRename2
(error) ERR source and destination objects are the same
127.0.0.1:6379> RENAME testRename2 testStr11
OK
127.0.0.1:6379> GET testStr11
"rename1"
127.0.0.1:6379> 
```
##RENAMENX :重命名这个新名称不存在才会生效
语法: RENAMENX key new key
SET testRename 'test'
RENAMENX testRename Six

```127.0.0.1:6379> RENAMENX testRename Six
(integer) 1
127.0.0.1:6379> GET SIX
(nil)
127.0.0.1:6379> GET Six
"test"
127.0.0.1:6379> 
```
##DUMP :序列化给的的KEY 返回序列化之后的值
 SET testDump 'this is jankz'
 DUMP testDump

```127.0.0.1:6379>  SET testDump 'this is jankz'
OK
127.0.0.1:6379>  DUMP testDump
"\x00\rthis is jankz\x06\x00\x9f\x7f\x8a\xee\x10\xb3a("
127.0.0.1:6379> 
```
##RESTORE :反序列化
语法:RESTORE KEY tt1 value
RESTORE testDump1 tt1
RESTORE testDump1 0 "\x00\rthis is jankz\x06\x00\x9f\x7f\x8a\xee\x10\xb3a("


```127.0.0.1:6379> RESTORE testDump1 0 "\x00\rthis is jankz\x06\x00\x9f\x7f\x8a\xee\x10\xb3a("
OK
127.0.0.1:6379> GET testDump1
"this is jankz"
```

##MOVE 将当前数据库中的key移动到另外的数据库
语法:MOVE key dbId
SET testMove 'aaa'
SELECT 0
MOVE testMove

```127.0.0.1:6379> SELECT 0
OK
127.0.0.1:6379> SET testMove 'aaa'
OK
127.0.0.1:6379> GET testMove
"aaa"
127.0.0.1:6379> SELECT 1
OK
127.0.0.1:6379[1]> GET testMove
(nil)
127.0.0.1:6379[1]> SELECT 0
OK
127.0.0.1:6379> MOVE testMove 1
(integer) 1
127.0.0.1:6379> SELECT 1
OK
127.0.0.1:6379[1]> GET testMove
"aaa"
127.0.0.1:6379[1]> 
127.0.0.1:6379> SET testMove 'bbb'
OK
127.0.0.1:6379> SELECT 1
OK
127.0.0.1:6379[1]> MOVE testMove 0
(integer) 0

```
注意:当移动一个不存在的key不会成功 返回失败
当目录数据库中存在同名key的时候会移动失败


