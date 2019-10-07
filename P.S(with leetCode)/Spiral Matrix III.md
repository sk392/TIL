### Spiral Matrix III

요번엔 [Spiral Matrix III](https://leetcode.com/problems/spiral-matrix-iii/)요거를 풀어봐씁니다.

### Problem

On a 2 dimensional grid with R rows and C columns, we start at (r0, c0) facing east.

Here, the north-west corner of the grid is at the first row and column, and the south-east corner of the grid is at the last row and column.

Now, we walk in a clockwise spiral shape to visit every position in this grid. 

Whenever we would move outside the boundary of the grid, we continue our walk outside the grid (but may return to the grid boundary later.) 

Eventually, we reach all R * C spaces of the grid.

Return a list of coordinates representing the positions of the grid in the order they were visited.

 

Example 1:

```
Input: R = 1, C = 4, r0 = 0, c0 = 0
Output: [[0,0],[0,1],[0,2],[0,3]]
```
![dsd](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/24/example_1.png)

 

Example 2:


```
Input: R = 5, C = 6, r0 = 1, c0 = 4
Output: [[1,4],[1,5],[2,5],[2,4],[2,3],[1,3],[0,3],[0,4],[0,5],[3,5],[3,4],[3,3],[3,2],[2,2],[1,2],[0,2],[4,5],[4,4],[4,3],[4,2],[4,1],[3,1],[2,1],[1,1],[0,1],[4,0],[3,0],[2,0],[1,0],[0,0]]
```

![dsd](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/24/example_2.png)

 

Note:

```
1 <= R <= 100
1 <= C <= 100
0 <= r0 < R
0 <= c0 < C
```

### Solution

```

class Solution {
    fun spiralMatrixIII(R: Int, C: Int, r0: Int, c0: Int): Array<IntArray> {
        var interval = 1
        var vector = 1 // 1 or -1
        val result = MutableList(0) { intArrayOf() }
        var r1 = r0
        var c1 = c0

        result.add(intArrayOf(r1, c1))
        while (result.size < R * C) {
            for (i in 0 until interval) {
                c1 += vector
                addSpiralMatrix(R, C, r1, c1, result)
            }

            for (i in 0 until interval) {
                r1 += vector
                addSpiralMatrix(R, C, r1, c1, result)
            }

            interval++

            vector *= -1
        }

        return result.toTypedArray()
    }

    private fun addSpiralMatrix(R: Int, C: Int, r1: Int, c1: Int, result: MutableList<IntArray>) {
        if (r1 in 0 until R && c1 in 0 until C) {
            result.add(intArrayOf(r1, c1))
        }

    }
}

```

### Explanation

처음엔 끝에 닿았을 때 분기 태워서 계산 해주려고 했으나, 예제 1번처럼 3바퀴를 돌려야될 때는 계산이 복잡해진다고 느껴졌다.. 그래서 그냥 스파이럴로 돌리는데 주어진 사이즈 안에 있으면 리스트에 넣도록 수정! 더 빠른 방법은 아마 맨 앞에 설명했던 방법이지 않을까!