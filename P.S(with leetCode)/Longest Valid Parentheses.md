### Longest Valid Parentheses

오늘의 문제는 Hard - [Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)

### Problem


Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

Example 1:

```
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"
```

Example 2:

```
Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"
```

### Solution

```
import java.util.*

class Solution {
    fun longestValidParentheses(s: String): Int {
        if (s.length in 0..1) return 0
        var longestResult = 0
        var startIndex = -1

        val stack: ArrayDeque<Int> = ArrayDeque()

        for (index in 0 until s.length) {
            if (s[index] == '(') {
                stack.push(index)
            } else {
                if (stack.isNotEmpty()) {
                    stack.pop()

                    val length = if (stack.isNotEmpty()) {
                        index - stack.peek()
                    } else {
                        index - startIndex
                    }

                    longestResult = Math.max(longestResult, length)
                }else{
                    startIndex = index
                }
            }
        }

        return longestResult
    }
}
```

### Explanation

으 하드난이도라 그런지 좀 시간이 좀 걸렸는데, 결국 요는 열리는 것과 닫히는 것의 매핑이 잘 되느지 확인하는게 중요하다. '('를 스택에 넣고 하나씩 팝하면서 사이즈를 넣어주는데, 팝한 후에도 (가 남아있다면 해당 인덱스로 계산해주고 남아있지 않다면 별도로 저장해둔 startIndex로 계산해준다. startIndex가 실제 시작지점보다 1개 낮게 되는 이유는 (초기값이 -1인것도 같은이유) 길이를 계산하기 위해서다