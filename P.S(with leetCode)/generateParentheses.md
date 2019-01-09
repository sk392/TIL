### Generate Parentheses


요번엔 [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)요거를 풀어봐씁니다.

### Problem

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```
### Solution

```
class Solution {

    val result = mutableListOf<String>()

    fun generateParenthesis(n: Int): List<String> {

        calc(n, n, "")

        return result
    }

    fun calc(openCnt: Int, closeCnt: Int, s: String) {

        if (openCnt == 0 && closeCnt == 0) {
            result.add(s)
        } else if (openCnt == 0) {
            calc(openCnt, closeCnt - 1, "$s)")
        } else {
            calc(openCnt - 1, closeCnt, "$s(")
            if(openCnt<closeCnt){
                calc(openCnt, closeCnt - 1, "$s)")
            }
        }
    }
}
```

### Explanation

DFS(Depth First Search)로 해결해따 ㅋㅋㅋ 진짜 옛날엔 바로바로 떠올랐는데 요즘엔 고민해야 떠오르는듯.. 역시 공부는 끊임없이 해야댐..ㅠㅠ

* BFS(Breadth First Search)와 코드상의 차이는 BFS는 큐를 사용해서 해결한다
