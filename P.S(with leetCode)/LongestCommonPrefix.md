### LongestCommonPrefix


요번엔 [LongestCommonPrefix](https://leetcode.com/problems/longest-common-prefix/)요거를 풀어봐씁니다.

### Problem


Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

Example 1:

```
Input: ["flower","flow","flight"]
Output: "fl"
```
Example 2:

```
Input: ["dog","racecar","car"]
Output: ""
```
Explanation: There is no common prefix among the input strings.

Note:

All given inputs are in lowercase letters a-z.




### Solution

```
class Solution {
    fun longestCommonPrefix(strs: Array<String>): String {
        if(strs.isEmpty()){
            return ""
        }
        if(strs.size==1){
            return strs[0]
        }
        var result = ""
        val tempValue = strs[0]

        for (i in 1 .. tempValue.length) {
            val tempString = tempValue.substring(0, i)
            for (j in 0 until strs.size) {
                if (strs[j].length < i || strs[j].substring(0, i) != tempString) {
                    return result
                }
            }
            result = tempString
        }

        return result
    }
}
```



### Explanation

음.. 맨앞에 리턴처리 하는 구문까지 로직에 녹여내고 싶은데... 까다롭다! 나중에 다시 풀면 생각해보자!