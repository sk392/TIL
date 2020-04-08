### LRU Cache



Today's Problem is Medium - [LRU Cache](https://leetcode.com/problems/lru-cache/)

### Problem


Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and put.

get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
put(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

The cache is initialized with a positive capacity.

Follow up:
Could you do both operations in O(1) time complexity?

Example:

```
LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```


### Solution

```
class LRUCache(val capacity: Int) {
    private val map : MutableMap<Int, Int> = mutableMapOf()
    
    fun get(key: Int): Int {
        return if(map.containsKey(key)){
            val value = map[key]!!
            map.remove(key)
            map[key] = value
            value
        }else{
            -1
        }
    }

    fun put(key: Int, value: Int) {
        map.remove(key)
        map[key] = value

        if(map.size > capacity){
            map.remove(map.keys.first())
        }
    }
}


```

### Explanation

문제를 엄청 오랜만에 풀어서 그런지 감도 많이 떨어졌다.. 후..
일단 문제 자체는 쉽게 해결할 수 있다. LinkedList + Map을 사용해서 해결할 수도 있고.. 방법은 여러가지겠지만, O(1)로 해결하는게 까다로웠다.
 그 방법을 map에 remove,add하는 방법으로 가장 최근에 찾은 값은 keys에 가장 마지막 값에 있을 것이고 가장 옛날에 찾은 값은 자연스레 map.keys[0]에 있을 것이다. 때문에 사이즈가 넘치게 되면 해당 값을 지워주면 된다.