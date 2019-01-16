### UniquePaths

요번엔 [UniquePaths](https://leetcode.com/problems/unique-paths/)요거를 풀어봐씁니다.

### Problem
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?


Above is a 7 x 3 grid. How many possible unique paths are there?

Note: m and n will be at most 100.

Example 1:

```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right
```
Example 2:

```
Input: m = 7, n = 3
Output: 28
```

### Solution

```
class Solution {
fun uniquePaths(m: Int, n: Int): Int {
    //BottomUp 형식
    val result = Array(m){ IntArray(n)}
    for(i in 0 until  m){
        for (j in 0 until  n){
            if(i == 0 || j == 0) {
                result[i][j] = 1
                continue
            }
            result[i][j] = result[i-1][j] + result[i][j-1]
        }
    }

    return result[m-1][n-1]
        }
}
```

### Explanation

 최근엔 DP에 대한 감을 굳히기 위해서 계속 DP를 풀어보고 있다. 케빈과 DP에 대해 잠시 얘기를 했었는데, 설명하기 힘들었어서 여기에 끄적여본다. 역시 뭔가 새로운 개념을 잡을 땐 정의부터 공부해야 되는듯
 
[DP (Dynamic Programming)](!https://namu.wiki/w/%EB%8F%99%EC%A0%81%20%EA%B3%84%ED%9A%8D%EB%B2%95?from=Dynamic%20Programming) : 동적 계획법이라고도 하는 이 알고리즘 설계 기법은 "어떤 문제를 풀기 위해 그 문제를 더 작은 문제의 연장선으로 생각하고, 과거에 구한 해를 활용하는" 방식의 알고리즘을 총칭한다. (가장 심플하게 설명하면 '기억하며 풀기')

역시 설명은 갓무위키만한게 없다. 궁금하면 위 링크를 눌러보자!

DP에도 조금 다른식으로 접근할 수도 있다(보통은 그러지 않아도 된다) 위 솔루션에서는 BottomUp형태로 문제를 해결해뒀는데, 아래는 TopDown 방식으로 해결한 솔루션과 (DP와 관련없지만) 수학공식을 활용해서 풀어본 솔루션도 있다.

```
//TopDown 형식
class Solution {
fun uniquePaths(m: Int, n: Int): Int {
    uniquePath(m-1,n-1)
    return uniquePathsResult[Pair(m-1,n-1)]!!
}
fun uniquePath(m: Int, n: Int): Int {
    if(uniquePathsResult[Pair(m,n)] != null) return uniquePathsResult[Pair(m,n)]!!
    if( m ==0 || n == 0 ){
        uniquePathsResult[Pair(m,n)] = 1
        return uniquePathsResult[Pair(m,n)]!!
    }
    var result = 0
    if(m>0)result += uniquePath(m-1,n)
    if(n>0)result += uniquePath(m,n-1)
    uniquePathsResult[Pair(m,n)] = result
    return uniquePathsResult[Pair(m,n)]!!
}
}
```

```
//수학 공식을 통한 풀이
import java.math.BigInteger
class Solution {
fun uniquePaths(m: Int, n: Int): Int {
    var result = BigInteger.valueOf(1)

    for (i in 1..m + n - 2) {
        result = result.multiply(i.toBigInteger())
    }
    for (i in 1 until n) {
        result = result.divide(i.toBigInteger())
    }
    for (i in 1 until m) {
        result = result.divide(i.toBigInteger())
    }
    return result.toInt()
        }
}
```