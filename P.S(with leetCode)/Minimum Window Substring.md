### Minimum Window Substring


Today's Problem Hard - [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

### Problem

Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

Example:

```
Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
```

Note:

* If there is no such window in S that covers all characters in T, return the empty string "".

* If there is such window, you are guaranteed that there will always be only one unique minimum window in S.

### Solution

```
class Solution {
    fun minWindow(s: String, t: String): String {
        val map = mutableMapOf<Char, Int>()
        var count = t.length
        var left = 0
        var right = 0
        var result = ""

        t.forEach {
            map[it] = map.getOrDefault(it, 0) + 1
        }


        while (right <= s.length) {
            if (count == 0) {
                val char = s[left++]
                if (map.containsKey(char)) {
                    map[char] = map.getOrDefault(char, 0) + 1

                    if (map[char] ?: 0 > 0) count++
                }
            } else {
                if (right == s.length) return result
                val char = s[right++]
                if (map.containsKey(char)) {
                    map[char] = map.getOrDefault(char, 0) - 1

                    if (map[char] ?: 0 >= 0) count--
                }
            }

            if (count == 0 && (result.isEmpty() || right - left < result.length)) {
                result = s.slice(left until right)
            }
        }

        return result
    }
}
```

### Explanation

요것은.. ~~수류탄이여~~ 가아니고 문제는 짧지만 굉장히 까다로운 문제였다.. 하루 꼬박 고민해봤지만 빵꾸없는 솔루션은 나오지 않았다. 흑흑 까다로웠던 것들이 유독 많았다. t의 중복 문자는 어떻게 해결할지, t와 같은 문제를 가졌다는건 어떤식으로 판단할지, 그 중에서 가장 작은 값은 어떤식으로 찾을지, 그리고 그 모든 것을 O(n)으로 해결할 수 있을지. 

Discuss를 참고해 솔루션을 찾았는데, map<Char,Int>를 통해 t의 중복 문자들을 해결했고, 같은 문자를 가졌는지는 count를 통해서 확인하고, leftIndex, rightIndex를 구해서 최대 O(n)을 지키고, 문자열을 잘라 가장 작은 값인지 판단하는 로직이다.

로직을 보다보면 if(map.conatine(key)) 내부에 따로 if(map[char] ?: 0 > 0) count++ 이런식으로 구현되어 있는 것을볼 수 있다. 처음 보면엥 그냥 위에 if문제 조건을 추가하는 식으로 진행해서 해결해도 되지 않나 했는데 map에 값들이 -로도 들어갈 수 있어야한다는 점을 알아야한다. map의 값이 -가 되면 left에서 + 하더라도 조건에 만족할 수 있는 더 짧은 솔루션을 찾을 수 있게된다.