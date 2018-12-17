### ReverseInteger


요번엔 [ReverseInteger](https://leetcode.com/problems/reverse-integer/)요거를 풀어봐씁니다.

### Problem

Given a 32-bit signed integer, reverse digits of an integer.

Example 1:

```
Input: 123
Output: 321
```
Example 2:

```
Input: -123
Output: -321
```
Example 3:

```
Input: 120
Output: 21
```

Note:
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.
 

### Solution

```
import java.lang.StringBuilder
import kotlin.math.abs

class Solution {
    
    fun reverse(x: Int): Int {
        val result = StringBuilder()
        var calcX = abs(x)
        if(x<0){
            result.append("-")
        }
        result.append("${calcX%10}")
        while(calcX>=10){
            calcX /= 10
            result.append("${calcX%10}")
        }
        try{
            calcX = result.toString().toInt()
        }catch (e : NumberFormatException){
            calcX = 0
        }
        return  calcX

    }

}

```

### Explanation

약간.. 스택이나 다른 칭구들을 고민해봐쓰나 요칭구가 젤 빨라?보여서 ㅋㅋㅋ 이걸로 해결!