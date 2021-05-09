## Redis


Redis有哪些数据类型
参考：https://redis.io/topics/data-types 
有五中类型，分别是：
* Strings 简单字符串类型，一个key对应一个value。我们用的最多的就是这种。
* Lists   列表有序集合，一个key对应一个List
* Sets    无需集合，一个key对应一个Set集合
* Hashes  哈西集合，一个key对应一组kv
* Sorted sets(也就是zset) 有序SET，存放数据不重复，但是是有序的。以后者位置为准，前者被剔除掉。


下面我们可以测试下这些数据，我们通过2种方法，1.是redis命令，2是jedis，java的客户端。


```
Strings

127.0.0.1:6379> SET mykey "Hello"
OK
127.0.0.1:6379> GET mykey
"Hello"
127.0.0.1:6379>


jedis.set("key", "value");
String value = jedis.get("key");
```


```
Lists


redis> RPUSH mylist "one"
(integer) 1
redis> RPUSH mylist "two"
(integer) 2
redis> RPUSH mylist "three"
(integer) 3
redis> LRANGE mylist 0 0
1) "one"
redis> LRANGE mylist -3 2
1) "one"
2) "two"
3) "three"
redis> LRANGE mylist -100 100
1) "one"
2) "two"
3) "three"
redis> LRANGE mylist 5 10
(empty list or set)
redis> 
```

```
Sets

127.0.0.1:6379> sadd set_key a
(integer) 1
127.0.0.1:6379> sadd set_key b
(integer) 1
127.0.0.1:6379> sadd set_key c
(integer) 1
127.0.0.1:6379> smembers set_key
1) "c"
2) "a"
3) "b"
127.0.0.1:6379>
```


```
Hashes

redis> HSET myhash field1 "Hello"
(integer) 1
redis> HSET myhash field2 "World"
(integer) 1
redis> HGETALL myhash
1) "field1"
2) "Hello"
3) "field2"
4) "World"
redis> 
```

```
Sorted sets(Zset)

127.0.0.1:6379> zadd z_key 0 a
(integer) 1
127.0.0.1:6379> zadd z_key 1 b
(integer) 1
127.0.0.1:6379> zadd z_key 2 c
(integer) 1
127.0.0.1:6379> ZRANGEBYSCORE z_key 0 2
1) "a"
2) "b"
3) "c"
127.0.0.1:6379>
```