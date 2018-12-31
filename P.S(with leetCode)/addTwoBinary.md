### addTwoBinary


요번엔 [addTwoBinary](https://leetcode.com/problems/add-binary/)요거를 풀어봐씁니다.

### Problem
Given two binary strings, return their sum (also a binary string).

The input strings are both non-empty and contains only characters 1 or 0.

Example 1:

```
Input: a = "11", b = "1"
Output: "100"
```

Example 2:

```
Input: a = "1010", b = "1011"
Output: "10101"
```

### Solution

```
class Solution {
    fun addBinary(a: String, b: String): String {
        val result = StringBuilder()
        val tempA = a.reversed()
        val tempB = b.reversed()
        var plusCount = 0
        for(i in 0 until  Math.max(a.length,b.length)){
            var temp= plusCount
            plusCount =0
            if( i<a.length){
                temp += tempA[i].toString().toInt()
            }
            if( i<b.length){
                temp += tempB[i].toString().toInt()
            }
            if(temp>=2){
                plusCount = 1
                temp -=2
            }
            result.append(temp)
        }
        if(plusCount!= 0){
            result.append(plusCount)
        }

        return result.toString().reversed()
    }
}
```

### Explanation

앜ㅋㅋㅋㅋㅋㅋㅋㅋㅋ 와.. 정말 하위 0퍼센트라니 대단해!!ㅋㅋㅋㅋㅋㅋㅋ 낼 다시 해봐야지..ㅋㅋㅋ