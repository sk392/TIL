### Longest Palindromic Substring



오늘의 문제는!! [Longest Palindromic Substring](https://leetcode.com/problems/all-possible-full-binary-trees/)

### Problem

Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

Example 1:

```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

Example 2:

```
Input: "cbbd"
Output: "bb"
```


### Solution

```
class Solution {
    fun longestPalindrome(s: String): String {
        if (s.isEmpty()) return ""

        val chars = s.toCharArray()
        val map = mutableMapOf<Int, MutableList<Pair<Int, Int>>?>()

        //find size 2 & 3
        for (i in 0 until chars.size) {
            if (i + 1 < chars.size && chars[i] == chars[i + 1]) {
                if (map[2] == null) map[2] = mutableListOf()

                map[2]!!.add(Pair(i, i + 1))
            }
            if (i + 2 < chars.size && chars[i] == chars[i + 2]) {
                if (map[3] == null) map[3] = mutableListOf()

                map[3]!!.add(Pair(i, i + 2))
            }
        }

        if (map.isEmpty()) return chars[0].toString()

        if (chars.size >= 4) {
            for (i in 4..chars.size) {
                map[i - 2]?.forEach {
                    val start = it.first - 1
                    val end = it.second + 1
                    if (start >= 0 && end < chars.size && chars[start] == chars[end]) {
                        if (map[i] == null) map[i] = mutableListOf()
                        map[i]!!.add(Pair(start, end))
                    }
                }


            }
        }

        val resultKey = map.keys.max() ?: 0

        return s.substring(map[resultKey]?.get(0)?.first ?: 0, (map[resultKey]?.get(0)?.second ?: 0) + 1)
    }
}
```

### Explanation

음.. 문제는 2개일 때와 3개일 때 로직이 약간 다르다 3개는(정확히는 홀수는) 가운데에 무슨 값이 있어도 반복 값이 될 수 있기 떄문에, 2개와 3개의 솔류션을 구해놓고 그 뒤로는 같은 로직으로 양끝에 것이 같으면 점점 사이즈를 키워가는 식으로 해결했다! 근데 메모리랑 속도가 14퍼센트정도로 현저히 낮다.. 그냥 순차적으로 n제곱 만큼 실행하는 코드들보단 빠르긴한데.. 