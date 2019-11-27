###  Regular Expression Matching


오늘의 문제는 Hard - [CRegular Expression Matching](https://leetcode.com/problems/regular-expression-matching/)요거를 풀어봐씁니다.

### Problem

Given an input string (s) and a pattern (p), implement regular expression matching with support for '.' and '*'.

'.' Matches any single character.
'*' Matches zero or more of the preceding element.
The matching should cover the entire input string (not partial).

Note:

s could be empty and contains only lowercase letters a-z.
p could be empty and contains only lowercase letters a-z, and characters like . or *.

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
p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

Example 3:

```
Input:
s = "ab"
p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

Example 4:

```
Input:
s = "aab"
p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab".
```

### Solution

```
class Solution {
    fun isMatch(s: String, p: String): Boolean {
        if (s.isEmpty() && p.isEmpty()) return true

        val dp = List(s.length+1) { BooleanArray(p.length+1) }
        dp[0][0] = true

        for (i in 2..p.length) {
            dp[0][i] = dp[0][i - 2] && p[i - 1] == '*'
        }

        for (i in 1..s.length) {
            for (j in 1..p.length) {
                val sc = s[i-1]
                val pc = p[j-1]

                if(pc == '*'){
                    //never use * or use it at least once
                    dp[i][j] = dp[i][j-2] || (dp[i-1][j] && (p[j-2] == sc || p[j-2] == '.'))
                }else if (sc == pc || pc == '.'){
                    dp[i][j] = dp[i-1][j-1]
                }

            }
        }

        return dp[s.length][p.length]
    }
}
```

### Explanation

요 근래에 풀었던 문제중에 제일 어려운 문제인 것같다.. 결국 못풀어서 Discuss를 봐서 해결..! 나중에 다시 풀어봐야겠당..