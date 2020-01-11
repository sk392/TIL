### Rotting Oranges



Today's Problem is Easy - [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

### Problem

In a given grid, each cell can have one of three values:

the value 0 representing an empty cell;
the value 1 representing a fresh orange;
the value 2 representing a rotten orange.
Every minute, any fresh orange that is adjacent (4-directionally) to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange.  If this is impossible, return -1 instead.

 

Example 1:

![](https://assets.leetcode.com/uploads/2019/02/16/oranges.png)

```
Input: [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
```

Example 2:

```
Input: [[2,1,1],[0,1,1],[1,0,1]]
Output: -1
Explanation:  The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.
```

Example 3:

```
Input: [[0,2]]
Output: 0
Explanation:  Since there are already no fresh oranges at minute 0, the answer is just 0.
```

### Solution

```
import java.util.*

class Solution {
    fun orangesRotting(grid: Array<IntArray>): Int {
        var freshOranges = 0
        val rotQueue: Queue<Int> = ArrayDeque()
        val row = grid.size
        val column = grid[0].size
        var time = 0

        for (r in 0 until row) {
            for (c in 0 until column) {
                if (grid[r][c] == 2) {
                    rotQueue.add(r * column + c)
                }
                if (grid[r][c] == 1) {
                    freshOranges++
                }
            }
        }

        val dirs = arrayOf(intArrayOf(0,1),intArrayOf(0,-1),intArrayOf(1,0),intArrayOf(-1,0))
        while (freshOranges !=0 && rotQueue.isNotEmpty()) {
            for( i in 0 until rotQueue.size){
                val curr = rotQueue.poll()

                for(dir in dirs) {
                    val r = curr / column + dir[0]
                    val c = curr % column + dir[1]

                    if(r in 0 until row && c in 0 until column && grid[r][c] == 1){
                        grid[r][c] = 2
                        rotQueue.add(r* column + c)
                        freshOranges--
                    }
                }
            }

            time++
        }

        return if(freshOranges == 0) time else -1
    }
}
```

### Explanation

문제 자체는 간단하게 풀 수 있는데 놓칠 수 있는 부분이 여러곳 있어서 조심해야한다.
아무것도 신선한 오렌지만 있는 경우는 -1 아무 것도 없거나 썩은오렌지만 있는 경우는 0이다.

여기서 좌표 계산을 1개의 인트로 하는 방법을 알아냈는데 r * column + c 를 하는 경우 나누면 r 나머지하면 c값을 구할 수 있다. c값은 column보다 적기 때문에 나눠서 1이 나올 순 없고 이건 인트의 특징을 이용한 방법이라고 할 수 있겠다.