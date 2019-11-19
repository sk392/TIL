### Next Permutation



오늘의 문제는 Medium - [Next Permutation](https://leetcode.com/problems/next-permutation/)

### Problem

Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```


### Solution

```

class Solution {
    fun nextPermutation(nums: IntArray): Unit {
        if (nums.size in 0..1) return

        for (i in (nums.size - 2) downTo 0) {
            val prev = nums[i + 1]
            val value = nums[i]
            var minValue = Int.MAX_VALUE
            var minIndex = -1

            for (j in i + 1 until nums.size) {
                val tempValue = nums[j]
                if (tempValue in (value + 1)..(minValue - 1)) {
                    minValue = tempValue
                    minIndex = j
                }
            }

            if (prev > value) {
                nums[i] = minValue
                nums[minIndex] = value
                val list= nums.toMutableList().subList(i+1, nums.size)
                if(list.size>1){
                    list.sort()
                    for(j in 0 until list.size){
                        nums[j+i+1] = list[j]
                    }
                }
                return
            }
        }
        nums.sort()

    }
}

```

### Explanation

우선 문제를 이해하는데 시간이 좀 걸렸다. 경우의 수단위로 나누는데 가장 작은 수부터 하나씩 늘려간다 

```1,2,3,4 -> 1,2,4,3 -> 1,3,2,4 -> 1,3,4,2 -> ... ```

그래서 뒤에서부터 찾은 다음에 내림 차순으로 들어가는 포인트를 찾고, 그 후에 해당 포인트에서 뒤로가면서 포인트 보다 크고 가장 작은 값을 찾아서 스위칭한다. 그 다음에 스위칭한 포지션 뒤에 값들을 소트해서 결과를 바꿔주면 문제 해결!