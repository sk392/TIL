###  TransposeMatrix


요번엔 [TransposeMatrix](https://leetcode.com/problems/transpose-matrix/)요거를 풀어봐씁니다.

### Problem
Given a matrix A, return the transpose of A.

The transpose of a matrix is the matrix flipped over it's main diagonal, switching the row and column indices of the matrix.


```
Example 1:

Input: [[1,2,3],[4,5,6],[7,8,9]]
Output: [[1,4,7],[2,5,8],[3,6,9]]
```

```

Example 2:

Input: [[1,2,3],[4,5,6]]
Output: [[1,4],[2,5],[3,6]]
```


```
Note:

1 <= A.length <= 1000
1 <= A[0].length <= 1000
```

### Solution

```
class Solution {
    fun transpose(A: Array<IntArray>): Array<IntArray> {
        val temp = mutableListOf<MutableList<Int>>()
        for(i in 0 until A[0].size){
            temp.add(mutableListOf())
            for ( j in 0 until A.size){
                temp[i].add(A[j][i])
            }
        }

        return temp.map {
            it.toIntArray()
        }.toTypedArray()
    }
}```

### Explanation

주말 저녁엔 역시 간단한 ps가 최고;; 생각나는대로 해결!