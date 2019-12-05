### Group Anagrams


Today's Problem Medium-  [Combination Sum](https://leetcode.com/problems/combination-sum/)

### Problem

Given an array of strings, group anagrams together.

Example:

```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

### Solution

```
class Solution {
    fun groupAnagrams(strs: Array<String>): List<List<String>> {
        val map = mutableMapOf<String, MutableList<String>>()
        val result = mutableListOf<List<String>>()
        strs.forEach {
            val sorted = it.toCharArray().sorted().toString()
            if (map.containsKey(sorted)) {
                val list = map[sorted] ?: mutableListOf()
                list.add(it)
            } else {
                map[sorted] = mutableListOf(it)
            }
        }

        map.keys.forEach {
            result.add(map[it] ?: listOf())
        }
        return result
    }
}
```

### Explanation

역시 미디움에 정답률 높은건 한방에 푸는 것들도 많은 것같다 이정도면 생각 없이도 풀 수 있다.

스펠 구성이 같은지 판단하는게 포인트인데, 이걸 그냥 CharArray로 만들고 소팅해서 해결했다. 같은 스펠로 구성되어 있으면 같은 갚이 나올테니, 그러고 Map을 통해서 찾는데 시간 들이지 않게끔 만들어서 해결!