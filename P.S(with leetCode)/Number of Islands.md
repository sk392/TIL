### Number of Islands



Today's Problem is Medium - [Number of Islands](https://leetcode.com/problems/number-of-islands/)

### Problem


Given an m x n 2d grid map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

 

Example 1:

```Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

Example 2:

```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
``` 

Constraints:

```
m == grid.length
n == grid[i].length
1 <= m, n <= 300
grid[i][j] is '0' or '1'.
```

### Solution

```kotlin
class Solution {
    fun numIslands(grid: Array<CharArray>): Int {
        var islandsCount = 0
        for (i in grid.indices) {
            for (j in grid[0].indices) {
                if (grid[i][j] == '1'){
                    islandsCount++
                    searchIslands(grid, Pair(i,j))
                }
            }
        }
        return islandsCount
    }

    fun searchIslands(grid: Array<CharArray>, startPointer: Pair<Int, Int>) {
        val pointX = startPointer.first
        val pointY = startPointer.second

        if (pointX < 0 || pointY < 0 || pointX >= grid.size || pointY >= grid[0].size || grid[pointX][pointY] != '1') return

        grid[pointX][pointY] = '0'
        searchIslands(grid, Pair(pointX, pointY + 1))
        searchIslands(grid, Pair(pointX + 1, pointY))
        searchIslands(grid, Pair(pointX, pointY - 1))
        searchIslands(grid, Pair(pointX - 1, pointY))
    }
}
```

### Explanation

기본적인 룰은 1의 좌표를 모두 기억해서 해당 좌표에 상하좌우 연결된 좌표를 계속해서 체크해나아가는 것이다. 더이상 체크할 것이 없으면 처음 1과 연결된 모든 좌표는 체크되어 있을 것이다!

 처음엔 비짓맵이랑 좌표리스트를 들고 시작했는데, visitmap은 해당 좌표는 이제 invalid하다는걸 체크하기 위해서였는데, 해당 값을 1 -> 0으로 대체해도 같은 결과를 도출할 수 있어 메모리를 아낄 수 있다. 그렇게 되면 자연스럽게 좌표리스트도 없애고 좌표를 찾는 순간 바로 체크할 수 있어서 나은 솔루션이 될 수있다.