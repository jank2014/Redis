#Redis 事务
##MULTI 开启事务,事务块中的多条语句会按照顺序放入队列当中,然后由EXEC命令执行
FLUSHALL 清空

```
127.0.0.1:6379> FLUSHALL
OK
127.0.0.1:6379> KEYS *
(empty list or set)
127.0.0.1:6379>  
```
MULTI 开启事务
MULTI
INCR counter1
INCR counter1
INCR counter1
PING
GET counter1
##通过EXEC执行事务块的命令
EXEC 
```
127.0.0.1:6379>  MULTI
OK
127.0.0.1:6379> INCR counter1
QUEUED
127.0.0.1:6379> INCR counter2
QUEUED
127.0.0.1:6379> INCR counter3
QUEUED
127.0.0.1:6379> PING
QUEUED
127.0.0.1:6379> GET counter1
QUEUED
127.0.0.1:6379> EXEC
1) (integer) 1
2) (integer) 1
3) (integer) 1
4) PONG
5) "1"
```
##WATCH :监视一个或者多个key
如果在执行事务之前这个key如果被其他的命令改变,事务就被打断了
客户端1:
WATCH counter1 counter2
MULTI
INCR counter1
INCR counter2

```
127.0.0.1:6379> WATCH counter1 counter2
OK
127.0.0.1:6379> INCR counter1
(integer) 2
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> INCR counter1
QUEUED
127.0.0.1:6379> INCR counter2
QUEUED
```
客户端2 对counter1进行修改

```
127.0.0.1:6379> keys *
1) "counter3"
2) "counter2"
3) "counter1"
127.0.0.1:6379> SET counter1 10
OK
127.0.0.1:6379> GET counter1
"10"
127.0.0.1:6379> 
```
此时执行EXEC 事务被中断返回nil

```
127.0.0.1:6379> INCR counter1
(integer) 2
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> INCR counter1
QUEUED
127.0.0.1:6379> INCR counter2
QUEUED
127.0.0.1:6379> EXEC
(nil)
```

```
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> INCR counter1
QUEUED
127.0.0.1:6379> INCR counter2
QUEUED
127.0.0.1:6379> EXEC
1) (integer) 11
2) (integer) 2
```
##UNWATCH :取消WATCH对所有key的监视

##DISCARD :取消事务 
MULTI
SET testMulti1 'this is a test for multi'
INCR counter1
DISCARD

```
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> SET testMulti1 'this is a test for multi'
QUEUED
127.0.0.1:6379> INCR counter1
QUEUED
127.0.0.1:6379> INCR counter2
QUEUED
127.0.0.1:6379> DISCARD
OK
127.0.0.1:6379> GET testMulti1
(nil)
```
##事务错误处理
1.语法错误:像命令不存在或者参数错误.如果有语法错误redis 接到EXEC后直接返回错误,事务中正确的命令也不会执行MULTI
MULTI
SET test jankz
SET test1
ERRORCOMMAND

```
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> SET test jankz
QUEUED
127.0.0.1:6379> SET test1
(error) ERR wrong number of arguments for 'set' command
127.0.0.1:6379> ERRORCOMMAND
(error) ERR unknown command 'ERRORCOMMAND'
127.0.0.1:6379> EXEC
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> KEYS *
1) "counter3"
2) "counter2"
3) "counter1"
```
2.运行错误,错误指在运行命令的时候出现的问题
MULTI
SET test 2 1
SADD test2 2
SET test2 3

```
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> SET test2 1
QUEUED
127.0.0.1:6379> SADD test2 2
QUEUED
127.0.0.1:6379> SET test2 3
QUEUED
127.0.0.1:6379> EXEC
1) OK
2) (error) WRONGTYPE Operation against a key holding the wrong kind of value
3) OK
```
注意:Redis 事务失败 不会回滚

