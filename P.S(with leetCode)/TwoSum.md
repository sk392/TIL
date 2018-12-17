### TwoSum

저도 리트를 가볍게 시작을 한번...

요번엔 [TwoSum](https://leetcode.com/problems/two-sum/)요거를 풀어봐씁니다.

### Problem
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].

```
 

### Solution

```
class Solution {
    fun twoSum(nums: IntArray, target: Int): IntArray {
        val calcMap: MutableMap<Int, Int> = HashMap()
        var result = intArrayOf()

        for (i in 0 until nums.size-1) {
            calcMap.put(target - nums[i], i)
        }

        nums.forEachIndexed { index, i ->
            if (calcMap.containsKey(i) && calcMap.get(i) != index) {
                result = intArrayOf(calcMap.get(i)!!, index)
            }
        }
        return result
    }
}

```

### Explanation

이 문제는 단순히 반복문을 이중으로 돌려도 풀 수 있지만, 포인트는 2개의 조합 숫자로 target을 만들어내고, 정답이 딱 1케이스만 있다는 것이 중요하다. A + B = C로 딱 한케이스 인데, 이 때 B = C - A라는 점을 보고 

```
for (i in 0 until nums.size-1) {
            calcMap.put(target - nums[i], i)
        }
```

이렇게 B에 대해서 맵을 구성한 후에 Key를 값으로 사용하고, Value를 Index를 사용한다. (결국 알고 싶은 것은 Index니까) 그 후에 C와 A를 가지고 B를 찾는 과정을 한 번 도는데,

```
nums.forEachIndexed { index, i ->
            if (calcMap.containsKey(i) && calcMap.get(i) != index) {
                result = intArrayOf(calcMap.get(i)!!, index)
            }
        }
```

C-A라는 값이 맵에 존재하고, 그 값(Index)이 자기 자신의 Index와 같지 않을 때 해당 결과가 정답이 된다.

총 2N의 시간이 걸리고 N만큼의 공간을 필요로 한다.