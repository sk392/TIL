###  Number of Equivalent Domino Pairs


요번엔 [ Number of Equivalent Domino Pairs](https://leetcode.com/problems/number-of-equivalent-domino-pairs/)요거를 풀어봐씁니다.

### Problem
Given a list of dominoes, dominoes[i] = [a, b] is equivalent to dominoes[j] = [c, d] if and only if either (a==c and b==d), or (a==d and b==c) - that is, one domino can be rotated to be equal to another domino.

Return the number of pairs (i, j) for which 0 <= i < j < dominoes.length, and dominoes[i] is equivalent to dominoes[j].

 

Example 1:

```
Input: dominoes = [[1,2],[2,1],[3,4],[5,6]]
Output: 1
``` 

Constraints:

```
1 <= dominoes.length <= 40000
1 <= dominoes[i][j] <= 9
```

### Solution

```
class Solution {
    fun numEquivDominoPairs(dominoes: Array<IntArray>): Int {
        val map = mutableMapOf<String, Int>()
        var result = 0
        dominoes.forEach {
            val a = it.sortedArray().toList().toString()
            map[a] = (map[a] ?: 0) + 1
        }
        map.values.forEach {
            result += it * (it - 1) / 2
        }
        return result
    }
}
```

### Explanation

간단한 문제! 해결잼!