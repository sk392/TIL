###  Letter Combinations of a Phone Number


오늘의 문제는! [Letter Combinations of a Phone Number](https://leetcode.com/problems/distant-barcodes/)

### Problem

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![이미지](http://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

Example:

```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

Note:

```
Although the above answer is in lexicographical order, your answer could be in any order you want.
```

### Solution

```
class Solution {
    fun letterCombinations(digits: String): List<String> {
        var results = mutableListOf<String>()

        var index = 0
        while (index < digits.length) {
            val list = getStringDigits(digits[index])
            if (results.isEmpty()) {
                list.forEach {
                    results.add(it)
                }
            } else {
                val newList = mutableListOf<String>()

                results.forEach { result ->
                    list.forEach {
                        newList.add(result + it)
                    }
                }

                results = newList
            }

            index++
        }
        return results
    }

    fun getStringDigits(c: Char): List<String> {
        return when (c) {
            '2' -> listOf("a", "b", "c")
            '3' -> listOf("d", "e", "f")
            '4' -> listOf("g", "h", "i")
            '5' -> listOf("j", "k", "l")
            '6' -> listOf("m", "n", "o")
            '7' -> listOf("p", "q", "r", "s")
            '8' -> listOf("t", "u", "v")
            '9' -> listOf("w", "x", "y", "z")
            else -> emptyList()
        }
    }
}
```

### Explanation

역시 미디움문제가 딱 풀기가 좋은듯.. 이게좀.. DP형탠가..? BFS형탠가..? 약간 헷갈리긴하는데 쩄든 재귀형태를 사용하면 오버플로우가 날 것같아서 반복문으로 해결했다. 숫자가 하나씩 나아갈 때마다 해당 알파벳을 붙여주면서 나아가면 해결! 속도가 75%정도 나왔는데 100퍼센트느 멀까