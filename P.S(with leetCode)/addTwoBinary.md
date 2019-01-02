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
     BigInteger(a,2).add(BigInteger(b,2)).toString(2)
         }
}
```

### Explanation

앜ㅋㅋㅋㅋㅋㅋㅋㅋㅋ 와.. 정말 하위 0퍼센트라니 대단해!!ㅋㅋㅋㅋㅋㅋㅋ 낼 다시 해봐야지..ㅋㅋㅋ

-----
 엥;;; 아니.. 이렇게도 풀 수 있구나... 먼가 현타가...ㅋㅋㅋㅋㅋㅋ 심지어 100퍼센트야;