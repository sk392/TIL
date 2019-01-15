###  ClimbingStairs


요번엔 [ClimbingStairs](https://leetcode.com/problems/climbing-stairs/)요거를 풀어봐씁니다.

### Problem

You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Note: Given n will be a positive integer.

Example 1:

```Input: 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

Example 2:


```
Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

### Solution

```
class Solution {

    var set = HashMap<Int,Int>()
    fun climbStairs(n: Int): Int {
        return compute(n,0)
    }
    fun compute(n: Int, result : Int) : Int {
        if(set[n] != null ) return set[n]!!
        var temp = result
        if(n <= 0) return 1
        else {
            temp += compute(n-1,result)
            if(n>=2) temp += compute(n-2,result)
            set[n] = temp
            return temp
        }

    }
}
```

### Explanation

어제 DP문제를 풀고 좀 더 DP를 여러개 풀어봐야겠다는 생각에 다른 문제도 풀어보았다. 그냥 간단한 DP로 풀면 시간 초과가 나므로 피보나치 때를 생각해서 값을 기억하고 있는 set을 만들어 한 번 연산 된 n에 대해서는 다시 연한하지 않도록 수정해서 통과!