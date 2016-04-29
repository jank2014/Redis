# Redis
Redis
##Redis 从零到入门

Redis
远程服务器字典

homebrew 安装的
```
To have launchd start redis now and restart at login:
  brew services start redis
Or, if you don't want/need a background service you can just run:
  redis-server /usr/local/etc/redis.conf
```

##bin目录可执行程序
* redis-server Redis服务器
* redis-cli 客户端
* redis-benchmark 性能测试工具
* redis-check-aof AOF文件修复工具
* redis-check-dump RDB文件检测工具
* redis.conf Redis的配置文件

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

```
jankz:~ jankz$ redis-cli
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

```
1) "loglevel"
2) "notice"
```
CONFIG GET port

```
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

* EX seconds 设置键的key的过期时间
* PX milliseconds :以毫秒的形式设置过期时间 SET key value PX milliseconds-PSETEX
* NX 只有键不存在的时候才可以设置成功SET key value NX -SETNX
* XX 只有key已经存在的时候才可以设置
可以自由组合

## #计数相关
####INCR KEY  对key中存储的数字加1
SET counter 1
INCR counter
key如果不存在会初始化为0再进行INCR操作
#### INCRBY  num 对key中存储的数字加num INCRBY不带空格
SET counter2 10
INCR BY counter2 16

```
127.0.0.1:6379> INCR BY counter2 10
(error) ERR wrong number of arguments for 'incr' command
127.0.0.1:6379> INCRBY counter2 10
(integer) 20
127.0.0.1:6379>  

```
####INCRBYFLOAT 加浮点数
SET float1 4.2
INCRBYFLOAT float1 3.4

```
127.0.0.1:6379>  SET float1 4.2
OK
127.0.0.1:6379> INCRBYFLOAT float1 3.4
"7.6"
```
####DECR 将key中存储数字减1
DECR counter2
同名会覆盖
####DECRBY J将key中存储的数值减去指定的值

```
127.0.0.1:6379> DECR counter2
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

```
127.0.0.1:6379> SET testStr  = 'jankz'
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

* GETRANGE testStr2 0 4 
* GETRANGE testStr2 0 -3
* GETRANGE testStr2 -4 -2
* GETRANGE testStr2 0 1000
* GETRANGE testStr2 -4 2

```
127.0.0.1:6379> SET testStr2 'maiziedu'
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
 
*  SET testStr3 'King' 
*  GET testStr3
*  SET testStr3 'Queen' 
*  GETSET testStr3 'King'
 
```
127.0.0.1:6379> SET testStr3 'King'
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

```
127.0.0.1:6379> SET testStr9 'Hello Jankz'
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

```
127.0.0.1:6379> APPEND testStr11 'edu'
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
* HSET key FIELD value
* HSET userInfo1 username 'king'
* HSET userInfo1 password '123456'
* HSET userInfo1 email 'jankz@jankz.com'

```
127.0.0.1:6379> APPEND noExistsTest 'ajax'
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
* HGET userInfo1 username
* HGET userInfo1 password
* HGET userInfo1 email

```
127.0.0.1:6379> HGET userInfo1 username
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

```
127.0.0.1:6379> HSETNX testhash1 test 'a'
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

```
127.0.0.1:6379> HMGET userInfo2 username nickname password email
1) "jankz"
2) "jz"
3) "123456"
4) "jankz@jankz.com"
```

HMGET userInfo3 a b c

```
127.0.0.1:6379> HMGET userInfo3 a b c
1) (nil)
2) (nil)
3) (nil)
```
注意:如果哈希表key中field不存在会返回nil

###HGETALL 返回哈希表key中所有的field和value
语法HGETALL key

```
127.0.0.1:6379> HGETALL userInfo2
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

```
127.0.0.1:6379> HKEYS userInfo2
1) "username"
2) "nickname"
3) "password"
4) "email"
```
###HVALS key 返回hash中key中所有的对应的值
语法 HVALS KEY 
HVALS userInfo2
  
```
127.0.0.1:6379> HVALS userInfo2
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

```
127.0.0.1:6379> HSET userInfo3 age 12
(integer) 1
127.0.0.1:6379> HINCRBY userInfo3 age 10
(integer) 22 
```
###HINCRBYFLOAT 加浮点数

语法 HINCRBYFLOAR KEY FIELD increment

HSET userInfo3 salary 1200
HINCRBYFLOAT userInfo3 salary 10.11

```
127.0.0.1:6379> HSET userInfo3 salary 1200
(integer) 1
127.0.0.1:6379> HINCRBYFLO
```

###HDEL :删除hash中key的指定域
语法 :HDEL KEY FIELD FIELD 
注意: 
HDEL userInfo2 username password

##LIST


#和key相关的命令
##KEYS 返回所有符合给定模式的key
* \* : 匹配人一个字符
* ? :匹配任意一个字符
* []:匹配[]之间的一个字符,[a-z]
* \x:匹配特殊字符\?\*;
* keys *
* keys *o*
* keys t??
* keys ?o*
* keys a[a-z]*
##EXISTS 检测指定key是否存在
语法:EXISTS key
EXISTS one
###TYPE :返回key所存储的类型
TYPE userInfo2

```
127.0.0.1:6379> TYPE userInfo2
hash
```
* 不存在返回none
* string
* hash
* list
* set 
* zset
##EXPIRE 设置key的过期时间
语法: EXPIRE key seconds
SET cache_page 'http://jzopen.com'
EXPIRE cache_page 100


```
127.0.0.1:6379> TYPE userInfo2
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
* 语法:PERSIST key 
* SET cache 'testCache'
* EXPIRE cache 100
* TTL cache
* PERSIST cache
* TTL cache

```
127.0.0.1:6379> SET cache 'testCache'
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
* DEL key

```
127.0.0.1:6379> DEL cache
(integer) 1
```

##RANDOMKEY 随机的从当前数据库中返回一个key
语法:RANDOMKEY

```
127.0.0.1:6379> RANDOMKEY
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

```
127.0.0.1:6379> SET testRename1 'rename1'
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

```
127.0.0.1:6379> RENAMENX testRename Six
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

```
127.0.0.1:6379>  SET testDump 'this is jankz'
OK
127.0.0.1:6379>  DUMP testDump
"\x00\rthis is jankz\x06\x00\x9f\x7f\x8a\xee\x10\xb3a("
127.0.0.1:6379> 
```
##RESTORE :反序列化
语法:RESTORE KEY tt1 value
RESTORE testDump1 tt1
RESTORE testDump1 0 "\x00\rthis is jankz\x06\x00\x9f\x7f\x8a\xee\x10\xb3a("

```
127.0.0.1:6379> RESTORE testDump1 0 "\x00\rthis is jankz\x06\x00\x9f\x7f\x8a\xee\x10\xb3a("
OK
127.0.0.1:6379> GET testDump1
"this is jankz"
```

##MOVE 将当前数据库中的key移动到另外的数据库
语法:MOVE key dbId
SET testMove 'aaa'
SELECT 0
MOVE testMove

```
s127.0.0.1:6379> SELECT 0
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


