###  Partition Labels


요번엔 [Partition Labels](https://leetcode.com/problems/partition-labels/)요거를 풀어봐씁니다.

### Problem
A string S of lowercase letters is given. We want to partition this string into as many parts as possible so that each letter appears in at most one part, and return a list of integers representing the size of these parts.

Example 1:

```
Input: S = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.
```

```
Note:

S will have length in range [1, 500].
S will consist of lowercase letters ('a' to 'z') only.
```

### Solution

```
class Solution {
    fun partitionLabels(s : String) : List<Int>{
        val res = arrayListOf<Int>()
        val lastPostions = IntArray(26){0}
        val chars = s.toCharArray()
        chars.forEachIndexed{index, value ->
            lastPostions[value-'a'] = index
        }
        var start =-1
        var end =-1
        chars.forEachIndexed { index, value ->
            if(start == -1) start=index
            end = Math.max(end,lastPostions[value-'a'])
            if(end== index){
                res.add(end-start+1)
                start = -1
            }
        }
        return res
    }
}
```

### Explanation

음.. 문제 자체를 잘 이해를 못했다가 솔루션을 보고나서 꺠달았당... 알고리즘 문제푸는건 로직 잘 짜는것과 문제를 명확히 이해하는게 필요한듯!!