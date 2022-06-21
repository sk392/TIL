### Palindrome Partitioning


Today's Problem is Medium - [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

### Problem
Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.

A palindrome string is a string that reads the same backward as forward.

 

Example 1:

```
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
```

Example 2:

```
Input: s = "a"
Output: [["a"]]
``` 

Constraints:

```
1 <= s.length <= 16
s contains only lowercase English letters.
```

### Solution

```
class Solution {
    fun partition(s: String): List<List<String>> {
        val result = mutableListOf<List<String>>()

        recursivePartition(s, mutableListOf(), result)

        return result
    }

    private fun recursivePartition(s: String, list: MutableList<String>, result: MutableList<List<String>>) {
        val endIndex = s.length - 1

        for (i in s.indices) {
            val currentStr = s.slice(0..i)
            if (isPalindrome(currentStr)) {
                if (i == endIndex) {
                    list.add(currentStr)
                    result.add(list)
                } else {
                    recursivePartition(s.slice(i + 1..endIndex), mutableListOf<String>().apply {
                        addAll(list)
                        add(currentStr)
                    }, result)
                }
            }
        }
    }
    private fun isPalindrome(s: String): Boolean {
        return s == s.reversed()
    }
}
```

### Explanation

이런 경우의 수를 찾는 문제의 특징중에 하나인데, 재귀를 통해 각각의 케이스를 분리하여 생각하면 좋음. aab에서 palindrome을 찾는건 a를 떼고 ab에서 palindrome을 찾은 후에 다시 합치는 것과 같다 divide & conquer의 형태라 생각해도 좋다. 그 후에 모든 결과를 result라는 곳에 담고, 위 코드에서 result를 전역 변수로 빼면 간단해질 수 있다.