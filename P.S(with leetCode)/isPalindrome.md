### isPalindrome


요번엔 [TwoSum](https://leetcode.com/problems/palindrome-number/)요거를 풀어봐씁니다.

### Problem

Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

Example 1:

```
Input: 121
Output: true
```

Example 2:

```
Input: -121
Output: false
```

Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.

Example 3:

```
Input: 10
Output: false
```

Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
Follow up:

Coud you solve it without converting the integer to a string?

### Solution

```
class Solution {
    fun isPalindrome(x: Int): Boolean {
            val convertX = x.toString()
            for (i in 0 until convertX.length / 2 ) {
                if (convertX[i] != convertX[convertX.length - 1 - i]) {
                    return false
                }
            }
            return true
    }
}

```

### Description

곱하고 나누는 것보다 요게 빠른 것같아성... 요걸루 했다!