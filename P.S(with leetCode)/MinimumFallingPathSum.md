### Minimum Falling Path Sum



요번엔 [Minimum Falling Path Sum](https://leetcode.com/problems/minimum-falling-path-sum/)요거를 풀어봐씁니다.

### Problem

Given a square array of integers A, we want the minimum sum of a falling path through A.

A falling path starts at any element in the first row, and chooses one element from each row.  The next row's choice must be in a column that is different from the previous row's column by at most one.

 
```
Example 1:

Input: [[1,2,3],[4,5,6],[7,8,9]]
Output: 12
Explanation: 
The possible falling paths are:
[1,4,7], [1,4,8], [1,5,7], [1,5,8], [1,5,9]
[2,4,7], [2,4,8], [2,5,7], [2,5,8], [2,5,9], [2,6,8], [2,6,9]
[3,5,7], [3,5,8], [3,5,9], [3,6,8], [3,6,9]
```

The falling path with the smallest sum is [1,4,7], so the answer is 12.

 
```
Note:

1 <= A.length == A[0].length <= 100
-100 <= A[i][j] <= 100
```

### Solution

```
class Solution {
var minFallingPathSumResult = Array(100){(IntArray(100) {Int.MAX_VALUE})}

fun minFallingPathSum(A: Array<IntArray>): Int {
    A[0].forEachIndexed { index, value ->
        minFallingPathSumResult[0][index] = value
        minFallingPath(1,index,value,A)
    }
    return minFallingPathSumResult[A.size-1].min()!!
}

fun minFallingPath(outerIndex :Int, index : Int, sum : Int, A : Array<IntArray>){
    if(outerIndex>= A.size ) return

    for ( i in index-1..index+1){
        if(i>=0 && i<A[outerIndex].size && sum+A[outerIndex][i] <minFallingPathSumResult[outerIndex][i]){
            val tempSum = sum+A[outerIndex][i]
            minFallingPathSumResult[outerIndex][i] = tempSum
            minFallingPath(outerIndex+1,i,tempSum,A)
        }
    }
}
}

```

### Explanation

맨 처음엔 Min값 하나만 들고 DP로 풀었는데, 그렇게 푸니까 계산했던 부분을 다시 계산해야되는 경우가 있어서 이걸 사이즈 최대치 array로 값을 저장하고 Min이 아닌 경우엔 더이상 계산하지 않도록 수정헀다



#### Discuss 

Discuss에서 awesome한 [솔루션](!https://leetcode.com/problems/minimum-falling-path-sum/discuss/186666/C%2B%2BJava-4-lines-DP)을 보았다!
내가 푼 방식도 라인별로 진행하면서 해당 라인의 각 항목에 최솟값들을 저장하는 식인데 이렇게되면 이전의 값들은 사실상 의미가 없는 값이고 싱크하다는게 보장되어 있다면 가장 최근의 항목만 있으면 되는것을 좀 더 포커싱한 솔루션이다
```
concept 

[1, 2, 3]
[4, 5, 6] => [5, 6, 8]
[7, 8, 9] => [7, 8, 9] => [12, 13, 15]
```


```
//원본 자바 코드
public int minFallingPathSum(int[][] A) {
  for (int i = 1; i < A.length; ++i)
    for (int j = 0; j < A.length; ++j)
      A[i][j] += Math.min(A[i - 1][j], Math.min(A[i - 1][Math.max(0, j - 1)], A[i - 1][Math.min(A.length - 1, j + 1)]));
  return Arrays.stream(A[A.length - 1]).min().getAsInt();
}        

```

보고서 짜본 내 코틀린 수정 코드

```
fun minFallingPathSum(A: Array<IntArray>): Int {
    val resultLine = IntArray(A.size) { A[0][it] }
    for (i in 1 until A.size) {
        val tempLine = IntArray(resultLine.size) { resultLine[it] }
        for (j in 0 until A.size) { // A.size == A[0].size
            resultLine[j] = A[i][j] +
                    Math.min(tempLine[j], Math.min(tempLine[Math.max(0, j - 1)], tempLine[Math.min(A.size - 1, j + 1)]))
        }
    }
    return resultLine.min() ?: 0
}
```