### Rotate Image

Today's Problemis Medium - [Rotate Image](https://leetcode.com/problems/rotate-image/)

### Problem
You are given an n x n 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

Note:

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

Example 1:

```
Given input matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

rotate the input matrix in-place such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

Example 2:

```
Given input matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

rotate the input matrix in-place such that it becomes:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```

### Solution

```
class Solution {
    fun rotate(matrix: Array<IntArray>) {
        var n = 0
        val size = matrix.size
        val s = size - 1

        while (n < (size / 2)) {
            for (x in n until size - 1 - n) {
                val temp = matrix[s - x][n]

                matrix[s - x][n] = matrix[s - n][s - x]
                matrix[s - n][s - x] = matrix[x][s - n]
                matrix[x][s - n] = matrix[n][x]
                matrix[n][x] = temp
            }
            n++
        }
    }
}
```

### Explanation

음 이 문제는 수학 공식처럼 해서 풀었다...ㅋㅋㅋ 바뀌는 좌표의 값을 전체 뎁스(바깥부터 몇번째의 라인인지) n과 마지막 인덱스값인 s, 해당 뎁스에 몇 번째 숫자인지 확인하는 x를 통해서 4각 형의 규칙으로 좌표를 계산함.

(n,x) -> (x,s-n) -> (s-n,s-x) -> (s-x,n) 으로 전환된다는 것을 알 수 있다.