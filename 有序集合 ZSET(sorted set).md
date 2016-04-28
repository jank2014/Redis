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

