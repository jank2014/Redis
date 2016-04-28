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

