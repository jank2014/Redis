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


