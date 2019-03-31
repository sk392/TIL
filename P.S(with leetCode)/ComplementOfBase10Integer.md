###  Complement Of Base 10 Integer


요번엔 [Complement Of Base 10 Integer](https://leetcode.com/problems/complement-of-base-10-integer/)요거를 풀어봐씁니다.

### Problem
Every non-negative integer N has a binary representation.  For example, 5 can be represented as "101" in binary, 11 as "1011" in binary, and so on.  Note that except for N = 0, there are no leading zeroes in any binary representation.

The complement of a binary representation is the number in binary you get when changing every 1 to a 0 and 0 to a 1.  For example, the complement of "101" in binary is "010" in binary.

For a given number N in base-10, return the complement of it's binary representation as a base-10 integer.


```
Example 1:

Input: 5
Output: 2
Explanation: 5 is "101" in binary, with complement "010" in binary, which is 2 in base-10.
```

```
Example 2:

Input: 7
Output: 0
Explanation: 7 is "111" in binary, with complement "000" in binary, which is 0 in base-10.
```

```
Example 3:

Input: 10
Output: 5
Explanation: 10 is "1010" in binary, with complement "0101" in binary, which is 5 in base-10.
```

```
Note:

0 <= N < 10^9
```

### Solution

```
class Solution {
    fun bitwiseComplement(N: Int): Int {
        var n = N
        if (n == 0) return 1
        if (n == 1) return 0
        var sum = 0
        var i = 0

        while (n > 1) {
            if (n % 2 == 0) {
                sum += Math.pow(2.toDouble(),i.toDouble()).toInt()
            }
            n /= 2
            i++
        }

        return sum
    }
}
```

### Explanation

주말이라 간단한 문제로 풀어봤다 진수변환 없이 바로바로 보어에 해당 하는 값을 구해서 더해주면 완료!