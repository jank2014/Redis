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

