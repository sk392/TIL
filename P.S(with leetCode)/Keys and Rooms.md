### Keys and Rooms


요번엔 [Keys and Rooms](https://leetcode.com/problems/keys-and-rooms/)요거를 풀어봐씁니다.

### Problem
There are N rooms and you start in room 0.  Each room has a distinct number in 0, 1, 2, ..., N-1, and each room may have some keys to access the next room. 

Formally, each room i has a list of keys rooms[i], and each key rooms[i][j] is an integer in [0, 1, ..., N-1] where N = rooms.length.  A key rooms[i][j] = v opens the room with number v.

Initially, all the rooms start locked (except for room 0). 

You can walk back and forth between rooms freely.

Return true if and only if you can enter every room.

```
Example 1:

Input: [[1],[2],[3],[]]
Output: true
Explanation:  
We start in room 0, and pick up key 1.
We then go to room 1, and pick up key 2.
We then go to room 2, and pick up key 3.
We then go to room 3.  Since we were able to go to every room, we return true.
```

```
Example 2:

Input: [[1,3],[3,0,1],[2],[0]]
Output: false
Explanation: We can't enter the room with number 2.
Note:

1 <= rooms.length <= 1000
0 <= rooms[i].length <= 1000
The number of keys in all rooms combined is at most 3000.
```


### Solution

```

//DFS Version.
import java.util.*

class Solution {
    val canVisitAllRoomsMap = HashSet<Int>()

    fun canVisitAllRooms(rooms: List<List<Int>>): Boolean {
        enterRooms(0,rooms)

        return canVisitAllRoomsMap.size == rooms.size
    }
    fun enterRooms(roomId : Int, rooms : List<List<Int>>){
        canVisitAllRoomsMap.add(roomId)
        rooms[roomId].forEach{key ->
            if (!canVisitAllRoomsMap.contains(key)) {
                enterRooms(key,rooms)
            }
        }
    }

}

```


```

//BFS Version.
import java.util.*

class Solution {
	val canVisitAllRoomsQueue = LinkedList<Int>()
	val canVisitAllRoomsMap = HashSet<Int>()
	
	fun canVisitAllRooms(rooms: List<List<Int>>): Boolean {
		findKeys(rooms[0])
		canVisitAllRoomsMap.add(0)
		while (!canVisitAllRoomsQueue.isEmpty()) {
			findKeys(rooms[canVisitAllRoomsQueue.pop()])
		}
		return canVisitAllRoomsMap.size == rooms.size
	}
	
	fun findKeys(keys : List<Int>){
		keys.forEach { key ->
		if (!canVisitAllRoomsMap.contains(key)) {
			canVisitAllRoomsMap.add(key)
			canVisitAllRoomsQueue.add(key)
		}
	}
}


```

### Explanation

맨첨에 BFS로 큐를 사용한 형태로 풀었는데 100/100나와서;; 디스커스 봤는데 DFS로 푸신분들 코드도 참고로 만들어바따