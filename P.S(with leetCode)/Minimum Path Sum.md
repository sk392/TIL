### Minimum Path Sum




TOday's Problem is Medium - [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

### Problem

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

Example:

```
Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.
```


### Solution

```
class Solution {
    fun minPathSum(grid: Array<IntArray>): Int {
        val n = grid.size
        val m = grid[0].size

        for (i in 0 until n) {
            for (j in 0 until m) {
                val current = grid[i][j]
                var min = current
                if (i >= 1) {
                    min = grid[i - 1][j] + current
                }
                if (j >= 1) {
                    min = if (min > current) {
                        Math.min(grid[i][j - 1] + current, min)
                    } else {
                        grid[i][j - 1] + current
                    }
                }
                grid[i][j] = min
            }
        }

        return grid[n - 1][m - 1]
    }
}
```

### Explanation

DP방식으로 해결했다. 가장 기본적인 형태인 n*m형태의 어레이를 사용해서 풀서치해서 한단계씩 앞으로 나아가도록 했다. 오른쪽하단으로 이동해야하니 현재값 + 현재 위 값 or 현재 좌측 값을 합중에 작은 값이 현재 경로의 최솟값이 될테고, 한칸씩 최솟값을 구해가 끝에서 결과를 반환해주면 된다.