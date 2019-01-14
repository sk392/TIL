### WildcardMatching


요번엔 [WildcardMatching](https://leetcode.com/problems/wildcard-matching/)요거를 풀어봐씁니다.

### Problem

Given an input string (s) and a pattern (p), implement wildcard pattern matching with support for '?' and '*'.

'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
The matching should cover the entire input string (not partial).

Note:

s could be empty and contains only lowercase letters a-z.
p could be empty and contains only lowercase letters a-z, and characters like ? or *.
Example 1:

```
Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```
Example 2:

```
Input:
s = "aa"
p = "*"
Output: true
Explanation: '*' matches any sequence.
```
Example 3:

```
Input:
s = "cb"
p = "?a"
Output: false
Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.
```
Example 4:

```
Input:
s = "adceb"
p = "*a*b"
Output: true
Explanation: The first '*' matches the empty sequence, while the second '*' matches the substring "dce".
```
Example 5:

```
Input:
s = "acdcb"
p = "a*c?b"
Output: false
```

### Solution

```
class Solution {
    fun isMatch(s: String, p: String): Boolean {
         val dp = Array(s.length + 1) { BooleanArray(p.length + 1) }
        dp[0][0] = true

        p.forEachIndexed { i, c -> if (c == '*' && dp[0][i]) dp[0][i + 1] = true }

        s.forEachIndexed { i, c ->
            p.forEachIndexed{ j, ch ->
                if(c==ch || ch == '?') dp[i+1][j+1] = dp[i][j]
                else if( ch == '*'){
                    dp[i+1][j+1] = dp[i][j] || dp[i+1][j] || dp[i][j+1]
                }
            }
        }
        return dp[s.length][p.length]
    }
}
```

### Explanation

음.. 요 문제의 포인트는 \?(single)와 \*(sequence)의 나이스한 처리기 핵심인 것같다. 그냥 주먹구구로 하게되면 \*의 처리가 굉장히 까다로운데 ?와 \*로 조합할 수 있는 경우의 수를 다 막아보려다(몽총..) 이건 아니다 싶어서 문제를 보고 DFS로 해결할 수 있을 것같아서 처리했다. 그러고 Discussion에서 DP로 문제를 해결한 것을 보고 이게 더 깔끔하다고 생각되어 이걸로 해결!
