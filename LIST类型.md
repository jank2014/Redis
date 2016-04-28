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


