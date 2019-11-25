### Combination Sum


오늘의 문제는 Medium-  [Combination Sum](https://leetcode.com/problems/combination-sum/)

### Problem

Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

The same repeated number may be chosen from candidates unlimited number of times.

Note:

All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
Example 1:

```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```

Example 2:

```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```


### Solution

```
class Solution {
    val combinationSumResult = mutableListOf<List<Int>>()
    fun combinationSum(candidates: IntArray, target: Int): List<List<Int>> {
        combinationCalculate(candidates, target, mutableListOf(), 0)
        return combinationSumResult
    }

    fun combinationCalculate(candidates: IntArray, target: Int, list: MutableList<Int>, startIndex: Int) {
        when {
            target < 0 -> return
            target == 0 -> combinationSumResult.add(list.toList())
            else -> {
                for (i in startIndex until candidates.size) {
                    val value = candidates[i]
                    if (value <= target) {
                        list.add(value)
                        combinationCalculate(candidates, target - value, list, i)
                        list.removeAt(list.size - 1)
                    }
                }
            }
        }
    }
}
```

### Explanation

처음으로 문제 해결한 방식은 중복을 없애기위해서 리스트를 정렬하고, 또 리스트를 스트링형태로 전환한 후에 set으로 저장을 해서 해결했다. 그 후에 디스커스를 참고해서 보니 중복된 값은 없다고 헀으니 시작 인덱스를 정해서 그 이상의 인덱스에서만 작업하면 중복으로 발생하는 일이 없어질 거고 그래서 이렇게 코드가 탄생했다.