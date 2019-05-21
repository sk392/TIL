### Custom Sort String.kt

요번엔 [Custom Sort String.kt](https://leetcode.com/problems/custom-sort-string/)요거를 풀어봐씁니다.

### Problem
S and T are strings composed of lowercase letters. In S, no letter occurs more than once.

S was sorted in some custom order previously. We want to permute the characters of T so that they match the order that S was sorted. More specifically, if x occurs before y in S, then x should occur before y in the returned string.

Return any permutation of T (as a string) that satisfies this property.

```
Example :
Input:
S = "cba"
T = "abcd"
Output: "cbad"
Explanation:
"a", "b", "c" appear in S, so the order of "a", "b", "c" should be "c", "b", and "a".
Since "d" does not appear in S, it can be at any position in T. "dcba", "cdba", "cbda" are also valid outputs.
```


```
Note:

S has length at most 26, and no character is repeated in S.
T has length at most 200.
S and T consist of lowercase letters only.
```

### Solution

```
    fun customSortString(S: String, T: String): String {
    val map = HashMap<Char, Int>()
    S.toCharArray().forEachIndexed { value, char ->
        map[char] = value + 1
    }
    val result = StringBuilder()
    T.toCharArray().sortedBy { map[it]?: 26 }.map {
        result.append(it)
    }
    return result.toString()
```

### Explanation

글자별로 가중치를 맵에 저장한 후 정렬을 해당 맵 데이터 기준으로 하면끝! 근데 흠.. sortedBy가 성능이 좋지는 않은지 33퍼센트가 나왔다 더 빠르게 해볼 수 있을까