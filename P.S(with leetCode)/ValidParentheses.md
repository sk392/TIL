### ValidParentheses


요번엔 [ValidParentheses](https://leetcode.com/problems/valid-parentheses/)요거를 풀어봐씁니다.

### Problem

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Note that an empty string is also considered valid.

Example 1:

```
Input: "()"
Output: true
```
Example 2:

```
Input: "()[]{}"
Output: true
```
Example 3:

```
Input: "(]"
Output: false
```
Example 4:

```
Input: "([)]"
Output: false
```
Example 5:

```
Input: "{[]}"
Output: true
```

### Solution

```
import java.util.*

class Solution {
    fun isValid(s: String): Boolean {
       val stack : Stack<Char> = Stack()
        var result = true
        for( i in 0 until s.length){
            if(isPop(s[i])){
                if(stack.size<=0 || stack.pop()!=s[i]){
                    return false
                }
            }else{
                stack.push(convert(s[i]))
            }
        }

        if(stack.size!=0){
            result =false
        }

        return result

    }
    fun isPop(c :Char)  = c == ')' || c == ']' || c == '}'
    fun convert(c : Char) = when(c){
        '(' -> ')'
        '[' -> ']'
        '{' -> '}'
        else -> null
    }
}
```

### Explanation
 
 이 문제의 요는 가장 최근의 것을 닫을 때 자신과 같은게 닫히는지?를 판단하면 되는 것같다. 그래서 스택이 가장 적절해보였고, 팝을 하면서 자신과 같거나 사이즈가 0보다 크다면 계속해서 체크해나아가서 풀었다! 배열을 통해 사이즈 3개짜리 인트 배열을 통해 각 항목별로 열면 + 닫으면 -를 통해도 간단하게 확인 가능할 듯!!