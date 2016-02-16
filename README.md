# Redis-plus #

----------

**Redis-plus** is a redis fork that supports a few new commands on various data structures. Without these commands, if you want to do the same operations, you have to use either pipelines or Lua scripts. These commands are implemented in pure C inside redis, to lift the burden from developers. 

## Commands supported ##
    
**HPOP HKEY KEY** 

- Pop a key from Hashmap and return its value, same as HGET + HDEL

**LFIND LKEY STARTAT VALUE** 
 
- Find the first index for value VALUE in a list, starting from STARTAT, return the index

**LINSERTAT LKEY INDEX VALUE** 

- Insert value at index in a list

**LCOUNT LKEY VALUE** 

- Return the count of items with VALUE in list

**SXOR SKEY1 SKEY2** 

- Perform 'exclusive OR' on Set1 and Set2 and return the result set

***It is still work in progress, more suggestions/pull requests are welcome.***


## Commands in action ##

**HPOP**

	127.0.0.1:6380> hset hash1 key1 123
	(integer) 1
	127.0.0.1:6380> hset hash1 key2 456
	(integer) 1
	127.0.0.1:6380> hvals hash1
	1) "123"
	2) "456"
	127.0.0.1:6380> hpop hash1 key1
	"123"
	127.0.0.1:6380> hvals hash1
	1) "456"

**LFIND**

	127.0.0.1:6380> lpush list1 1 2 3
	(integer) 3
	127.0.0.1:6380> lfind list1 0 2
	(integer) 1
	127.0.0.1:6380> lfind list1 0 3
	(integer) 2
	127.0.0.1:6380> lfind list1 2 1
	(integer) -1  # not found after position 2

**LINSERTAT**

	127.0.0.1:6380> lpush list1 1 2 3 4
	(integer) 4
	127.0.0.1:6380> linsertat list1 0 99
	(integer) 5
	127.0.0.1:6380> linsertat list1 3 88
	(integer) 6
	127.0.0.1:6380> lrange list1 0 -1
	1) "99"
	2) "4"
	3) "3"
	4) "88"
	5) "2"
	6) "1"

**LCOUNT**

	127.0.0.1:6380> rpush list2 1 2 3 3 4 4 4 5
	(integer) 8
	127.0.0.1:6380> lcount list2 4
	(integer) 3
	127.0.0.1:6380> lcount list2 22
	(integer) 0

	
**SXOR**

	127.0.0.1:6380> sadd set1 1 2 3
	(integer) 3
	127.0.0.1:6380> sadd set2 2 3 4 5
	(integer) 4
	127.0.0.1:6380> sxor set1 set2
	1) "1"
	2) "4"
	3) "5"

	

 
