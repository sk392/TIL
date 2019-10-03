### Positions of Large Groups

요번엔 [Positions of Large Groups](https://leetcode.com/problems/positions-of-large-groups/)요거를 풀어봐씁니다.

### Problem
In a string S of lowercase letters, these letters form consecutive groups of the same character.

For example, a string like S = "abbxxxxzyy" has the groups "a", "bb", "xxxx", "z" and "yy".

Call a group large if it has 3 or more characters.  We would like the starting and ending positions of every large group.

The final answer should be in lexicographic order.

 

Example 1:

```
Input: "abbxxxxzzy"
Output: [[3,6]]
Explanation: "xxxx" is the single large group with starting  3 and ending positions 6.
```
Example 2:

```
Input: "abc"
Output: []
Explanation: We have "a","b" and "c" but no large group.
```
Example 3:

```
Input: "abcdddeeeeaabbbcd"
Output: [[3,5],[6,9],[12,14]]
``` 

Note: 
```
  1 <= S.length <= 1000
```


### Solution

```
import java.util.Arrays
import java.util.ArrayList

class Solution {
    fun largeGroupPositions(S: String): List<List<Int>> {
        val largeGroup = ArrayList<List<Int>>()
        var i = 0

        while (i < S.length) {
            val temp = S[i]
            var j = i
            while (j < S.length && S[j] == temp) {
                j++
            }

            if (j - i >= 3) {
                largeGroup.add(Arrays.asList(i, j - 1))
            }
            i = j
        }

        return largeGroup
    }
}
```

### Explanation

간단하게 이지문제로 풀어보았따