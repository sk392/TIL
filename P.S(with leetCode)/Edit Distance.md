### Edit Distance



Today's Problem Hard - [Edit Distance](https://leetcode.com/problems/edit-distance/)

### Problem

Given two words word1 and word2, find the minimum number of operations required to convert word1 to word2.

You have the following 3 operations permitted on a word:

Insert a character
Delete a character
Replace a character
Example 1:

```
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```

Example 2:

```
Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```

### Solution

```
class Solution {
    fun minDistance(word1: String, word2: String): Int {
        val size1 = word1.length
        val size2 = word2.length
        val result = Array(size1 + 1) { index1 ->
            IntArray(size2 + 1) { index2 ->
                when {
                    index1 == 0 -> index2
                    index2 == 0 -> index1
                    else -> 0
                }
            }
        }

        for (i in 0 until size1) {
            for (j in 0 until size2) {
                result[i + 1][j + 1] = if (word1[i] == word2[j]) result[i][j] else 1 + minOf(result[i][j], minOf(result[i][j + 1], result[i + 1][j]))
            }
        }
        return result[size1][size2]
    }
}
```

### Explanation

오늘 문제 풀 때는 자력으로 풀려다 실패해서 공부하던 와중에 두 문자열의 비교를 찾는 레벤슈타인 알고리즘을 알게 되었다. 해당 알고리즘을 이용해서 문제 해결!