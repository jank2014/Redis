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


