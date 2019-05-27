### Jewels and Stones



요번엔 [Jewels and Stones](https://leetcode.com/problems/jewels-and-stones/)요거를 풀어봐씁니다.

### Problem
You're given strings J representing the types of stones that are jewels, and S representing the stones you have.  Each character in S is a type of stone you have.  You want to know how many of the stones you have are also jewels.

The letters in J are guaranteed distinct, and all characters in J and S are letters. Letters are case sensitive, so "a" is considered a different type of stone from "A".

Example 1:

```
Input: J = "aA", S = "aAAbbbb"
Output: 3
```

Example 2:

```
Input: J = "z", S = "ZZ"
Output: 0
```

Note:

```
S and J will consist of letters and have length at most 50.
The characters in J are distinct.
```

### Solution

```
class Solution {
    fun numJewelsInStones(J: String, S: String): Int {
            val map = HashMap<Char,Int>()

        S.toCharArray().forEach {
            map[it] = (map[it] ?: 0) + 1
        }
        var result = 0

        J.toCharArray().forEach { 
            result += (map[it] ?:0) 
        }
        return result
    }
}

```

### Explanation

월요일 아침이니 간단한 ez문제로!