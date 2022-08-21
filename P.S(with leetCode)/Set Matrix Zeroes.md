###  Set Matrix Zeroes

Medium - [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

### Problem
Given an m x n integer matrix matrix, if an element is 0, set its entire row and column to 0's.

You must do it in place.

Example 1:

![](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]
Output: [[1,0,1],[0,0,0],[1,0,1]]
Example 2:

![](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]]


Constraints:

```
m == matrix.length
n == matrix[0].length
1 <= m, n <= 200
-231 <= matrix[i][j] <= 231 - 1
``` 

Follow up:

```
A straightforward solution using O(mn) space is probably a bad idea.
A simple improvement uses O(m + n) space, but still not the best solution.
Could you devise a constant space solution?
```


### Solution

```kotlin
class Solution {
    fun setZeroes(matrix: Array<IntArray>): Unit {
        var isFirstZeroLineY = false
        for (x in matrix[0].indices) {
            if (matrix[0][x] == 0) {
                isFirstZeroLineY = true
            }
        }

        for (y in 1 until matrix.size) {
            var isZeroLineY = false
            for (x in matrix[0].indices) {
                if (matrix[y][x] == 0) {
                    isZeroLineY = true
                    matrix[0][x] = 0
                }
            }
            if (isZeroLineY) {
                for (x in matrix[0].indices) {
                    matrix[y][x] = 0
                }
            }
        }

        for (x in matrix[0].indices) {
            if (matrix[0][x] == 0) {
                for (y in 1 until matrix.size) {
                    matrix[y][x] = 0
                }
            } else if (isFirstZeroLineY) {
                matrix[0][x] = 0
            }
        }
    }
}
```

### Explanation

문제 자체는 O(M+N)으로 풀었다. X,Y에 각각 0의 위치를 저장해두었다가, 순차탐색하면서 각각의 X나 Y의 값과 같으면 0으로 채워주면 된다. 다만 마지막에 constant space로 해결하는 것은 어려워서 discuss의 도움을 받았다.
constant space로 해결하려면 기존에 있는 space를 쓰면된다. 첫 번째 행을 0을 채워야하는 열을 마킹하는 수단으로 사용하면되고, 이때 첫 번째 행에 0이 있으면 전부 0으로 바꿔줘야하는데, 이러면 마킹데이터가 의미가 없어지므로 플래그를 저장한 후에 가장 마지막에 첫 번째 라인을 채워주면 된다.
 N-Queens와 비슷하게, 각 0의 포지션을 라인별로 해결하면 좋은데, 라인별로 넘어갈 때 각행의 0이 있으면 0으로 다 채우고 다음 행으로 넘어가면된다. 다만 이 때 주의해야하는 점이 각 행의 0은 열을 마킹하는 값으로도 쓰이므로, 0이 존재한다면 우선 마킹을 전부 한 후에 해당 행을 0으로 채워줘야한다.
