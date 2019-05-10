### Max Increase to Keep City Skyline



요번엔 [Max Increase to Keep City Skyline](https://leetcode.com/problems/max-increase-to-keep-city-skyline/submissions/)요거를 풀어봐씁니다.

### Problem


In a 2 dimensional array grid, each value grid[i][j] represents the height of a building located there. We are allowed to increase the height of any number of buildings, by any amount (the amounts can be different for different buildings). Height 0 is considered to be a building as well. 

At the end, the "skyline" when viewed from all four directions of the grid, i.e. top, bottom, left, and right, must be the same as the skyline of the original grid. A city's skyline is the outer contour of the rectangles formed by all the buildings when viewed from a distance. See the following example.

What is the maximum total sum that the height of the buildings can be increased?

```
Example:
Input: grid = [[3,0,8,4],[2,4,5,7],[9,2,6,3],[0,3,1,0]]
Output: 35
Explanation: 
The grid is:
[ [3, 0, 8, 4], 
  [2, 4, 5, 7],
  [9, 2, 6, 3],
  [0, 3, 1, 0] ]

The skyline viewed from top or bottom is: [9, 4, 8, 7]
The skyline viewed from left or right is: [8, 7, 9, 3]

The grid after increasing the height of buildings without affecting skylines is:

gridNew = [ [8, 4, 8, 7],
            [7, 4, 7, 7],
            [9, 4, 8, 7],
            [3, 3, 3, 3] ]
```

```
Notes:

1 < grid.length = grid[0].length <= 50.
All heights grid[i][j] are in the range [0, 100].
All buildings in grid[i][j] occupy the entire grid cell: that is, they are a 1 x 1 x grid[i][j] rectangular prism.
```


### Solution

```
class Solution {
    fun maxIncreaseKeepingSkyline(grid: Array<IntArray>): Int {
        var result = 0
        val verticalSize = grid.size
        val horizontalSize = grid[0].size
        val maxVertical = MutableList(verticalSize){0}
        val maxLine = MutableList(horizontalSize){0}

        for(i in 0 until verticalSize){
            for (j in 0 until horizontalSize){
                maxLine[i] = Math.max(maxLine[i],grid[i][j])
                maxVertical[j] = Math.max(maxVertical[j],grid[i][j])
            }
        }

        for(i in 0 until verticalSize){
            for (j in 0 until horizontalSize){
                result +=Math.min(maxVertical[i],maxLine[j])-grid[i][j]

            }
        }
        return result
    }
}
```

### Explanation

쉬운문제여따!! 해당 열마다 가로 최고값, 세로 최고 값중에 최소값을 뽑아서 빼주면 끝!