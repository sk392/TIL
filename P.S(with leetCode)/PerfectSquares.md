### Perfect Squares


요번엔 [Perfect Squares](https://leetcode.com/problems/perfect-squares/)요거를 풀어봐씁니다.

### Problem

Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.

Example 1:

```
Input: n = 12
Output: 3
Explanation: 12 = 4 + 4 + 4.
```
Example 2:

```
Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
```
### Solution

```
class Solution {
    fun numSquares(n: Int): Int {
        val dp = IntArray(n+1)
        for(i in 1 .. n){
            var min = Int.MAX_VALUE
            var j = 1
            while(i - j*j >= 0){
                min = Math.min(min,dp[i-j*j] + 1)
                j++
            }
            dp[i] = min
        }
        return dp[n]
    }
}
```

### Explanation

처음엔 DFS로 하려다가;; 7284mm가 나왔다; 깜놀ㅋㅋㅋㅋㅋ 그래서 DP로 바꿨다! 200mm로 절감