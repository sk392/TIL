### Find and Replace Pattern



요번엔 [Find and Replace Pattern](https://leetcode.com/problems/find-and-replace-pattern/)요거를 풀어봐씁니다.

### Problem
You have a list of words and a pattern, and you want to know which words in words matches the pattern.

A word matches the pattern if there exists a permutation of letters p so that after replacing every letter x in the pattern with p(x), we get the desired word.

(Recall that a permutation of letters is a bijection from letters to letters: every letter maps to another letter, and no two letters map to the same letter.)

Return a list of the words in words that match the given pattern. 

You may return the answer in any order.

 
```
Example 1:

Input: words = ["abc","deq","mee","aqq","dkd","ccc"], pattern = "abb"
Output: ["mee","aqq"]
Explanation: "mee" matches the pattern because there is a permutation {a -> m, b -> e, ...}. 
"ccc" does not match the pattern because {a -> c, b -> c, ...} is not a permutation,
since a and b map to the same letter.
```
 
```
Note:

1 <= words.length <= 50
1 <= pattern.length = words[i].length <= 20
```

### Solution

```
class Solution {
fun findAndReplacePattern(words: Array<String>, pattern: String): List<String> {
    val resultArray = mutableListOf<String>()

    val resultPattern = getPattern(pattern)
    words.forEach {
        if (resultPattern.contentEquals(getPattern(it))) resultArray.add(it)
    }
    return resultArray
}

private fun getPattern(array: String) = IntArray(array.length).apply {
    val charMap = hashMapOf<Char,Int>()
    var maxValue = -1
    for (i in 0 until array.length) {
        val char = array[i]
        val value = charMap[char]

        if(value== null){
            maxValue++
            charMap[char] = maxValue
            this[i] = maxValue
        }else{
            this[i] = value
        }
    }
}

```

### Explanation

흠... 그래두 생각한대로 해서 40분 정도 걸린 것같다. 문자를 컨버팅해서 확인하는 것보단 숫자를 기준으로 패턴을 만드는형식으로 하면 편할 것같아서 이렇게 했다