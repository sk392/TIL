### RomanToInteger


요번엔 [RomanToInteger](https://leetcode.com/problems/roman-to-integer/)요거를 풀어봐씁니다.

### Problem

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9.
X can be placed before L (50) and C (100) to make 40 and 90.
C can be placed before D (500) and M (1000) to make 400 and 900.
Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

Example 1:

```
Input: "III"
Output: 3
```
Example 2:

```
Input: "IV"
Output: 4
```
Example 3:

```
Input: "IX"
Output: 9
```
Example 4:

```
Input: "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```
Example 5:

```
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

### Solution

```
class Solution {
    fun romanToInt(s: String): Int {
        var result = 0
        var tempValue = 0
        for (i in 0 until s.length) {
            val value = getNumberFromRoman(s[i])

            result += value
            if (value > tempValue) {
                result -= tempValue * 2
            }
            tempValue = value
        }
        return result
    }

    private fun getNumberFromRoman(c: Char) = when (c.toString()) {
        "I" -> 1
        "V" -> 5
        "X" -> 10
        "L" -> 50
        "C" -> 100
        "D" -> 500
        "M" -> 1000

        else -> 0
    }
}
```

### Explanation

로마 숫자의 규칙을 이해하면 쉽게 풀 수 있는 것같다.

로마 숫자는 기본적으로 숫자에 대응되는 문자를 더해줌으로서 값이 증가하는데 

"I" -> 1 , "II" -> 2, ...

예외의 경우는 "IV"와 같이 표현할 때이다. 로마숫자는 3개이상이 반복될 수 없기 떄문에 이렇게 표현하는데, 같은 유형의 표현들을 보면 "IX" , "XC" , "CM"과 같이 앞에 배치된 값이 뒤에 배치된 값보다 작다.

이러한 예외를 제외하면 로마 숫자는 왼쪽부터 오른쪽으로 갈수록 숫자가 작아지고, 다시 커지는 시점에서만("IX" 등)앞의 값을 빼주면 된다!