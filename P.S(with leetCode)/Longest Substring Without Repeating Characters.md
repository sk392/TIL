### Longest Substring Without Repeating Characters



오늘의 문제는!  [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

### Problem

Given a string, find the length of the longest substring without repeating characters.

Example 1:

```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```

Example 2:

```
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

Example 3:

```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```             
             
### Solution

```
class Solution {
    fun lengthOfLongestSubstring(s: String): Int {
		var longestLength = 0
        var subChars = mutableMapOf<Char, Int>()

        for (i in 0 until s.length) {
            val char = s[i]
            if (subChars.containsKey(char)) {
                val values = subChars.toList()
                val startIndex = (subChars[char] ?: 0)
                values.forEach {
                    if (it.second > startIndex) return@forEach

                    subChars.remove(it.first)
                }
            }
            subChars[char] = i

            longestLength = Math.max(longestLength, subChars.size)
        }

        return longestLength
    }
}
```

### Explanation

가장 긴 서브스트링을 구하는건데 겹치는 케릭터가 없이 구해야된다 맨처음엔 반복되는 구간이 없어야되는줄 알았는데 문제를 잘 못봤다; 반복문으로 순차적으로 시작해서 겹치는 캐릭터가 나오면 해당 인덱스보다 낮은 값들을 제거해주면 간단하게 해결! 근데 속도 및 메모리를 많이 쓰긴해서 다른 방법도 있는지 봐야지