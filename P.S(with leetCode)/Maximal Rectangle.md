### Maximal Rectangle


Today's Problem Hard - [Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)

### Problem

Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

Example:

```
Input:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
Output: 6
```

### Solution

```
class Solution {
    fun maximalRectangle(matrix: Array<CharArray>): Int {
        if(matrix.isEmpty()) return 0

        val n = matrix[0].size
        val left = IntArray(n)
        val right = IntArray(n) { n }
        val height = IntArray(n)
        var result = 0

        for (aMatrix in matrix) {
            var curLeft = 0
            var curRight = n
            for (j in 0 until n) {
                if (aMatrix[j] == '1') {
                    left[j] = Math.max(left[j], curLeft)
                    height[j]++
                } else {
                    left[j] = 0
                    curLeft = j + 1
                    height[j] = 0
                }
            }
            for (j in n - 1 downTo 0) {
                if (aMatrix[j] == '1') {
                    right[j] = Math.min(right[j], curRight)
                } else {
                    right[j] = n
                    curRight = j
                }
            }
            for (j in 0 until n) {
                result = Math.max(result, (right[j] - left[j]) * height[j])
            }
        }
        return result
    }

}
```

### Explanation

방법을 잘 모르겠어서 결국 discuss를 보고 풀었다. 추후에 다시 확인해보자!

일단 left좌표, right좌표, height를 각각 구해서 최종으로 값을 더해주면 된다.