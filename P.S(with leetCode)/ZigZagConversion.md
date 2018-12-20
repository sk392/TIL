### ZigZagConversion


요번엔 [ZigZagConversion](https://leetcode.com/problems/zigzag-conversion/)요거를 풀어봐씁니다.

### Problem

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

P   A   H   N
A P L S I I G
Y   I   R
And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

string convert(string s, int numRows);

Example 1:

```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```
Example 2:

```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:

P     I    N
A   L S  I G
Y A   H R
P     I
```

### Solution

```
class Solution {
    fun convert(s: String, numRows: Int): String {
        if(s.isEmpty()){
            return ""
        }
        if(numRows == 1){
            return s
        }
        val result = StringBuilder()
        for (i in 0 until numRows) {
            if(i>=s.length){
                break
            }
            var index = i
            result.append(s[index])
            while (index < s.length && result.length !=s.length) {

                if (numRows-1 - i > 0) {
                    if (index + (numRows-1 - i)*2 < s.length) {
                        index += (numRows-1 - i)*2
                        result.append(s[index])
                    } else {
                        break

                    }
                }

                if (i > 0) {
                    if (index + i*2 < s.length) {
                        index += i*2
                        result.append(s[index])

                    } else {
                        break
                    }
                }
            }

        }
        return result.toString()
    }
}
```



### Explanation

음...... 앞에 예외처리 하는 리턴은 알고리즘에서 빼기 어려운 것같다..ㅋㅋㅋㅋㅋㅋㅋ 빼기 귀찬잼... 앞에리턴이 있어야 속도가 줄어들긴한다!