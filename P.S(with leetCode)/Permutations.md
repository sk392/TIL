### Permutations


Today's Problem is Medium- [Permutations](https://leetcode.com/problems/permutations/)

### Problem

Given a collection of distinct integers, return all possible permutations.

Example:

```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```


### Solution

```
class Solution {
    fun permute(nums: IntArray): List<List<Int>> {
        val result = mutableListOf<List<Int>>()

        recurs(nums, mutableListOf(), result)
        return result
    }

    fun recurs(nums: IntArray, prev: List<Int>, result: MutableList<List<Int>>) {
        if(prev.size == nums.size){
            result.add(prev)
            return
        }

        for(i in 0 until nums.size){
            if(!prev.contains(nums[i])){
                val copyList = prev.toMutableList()
                copyList.add(nums[i])
                recurs(nums,copyList,result)
            }
        }

    }
}
```

### Explanation

간단한 문제긴한데.. 어려운 문제기도하다. 일전에 Next permutations라는 문제를 풀면서 감을 잡았다고 생각했고, 잘 풀었다. 근데, Discuss를 확인하고 놀람을 금치못했다.. 재귀로 풀면 엄청나게 쉽게 풀리는 것을...

1을 넣고 재귀 돌리고 1은 있으니까 2를 넣고 재귀, 3을 넣고 재귀 돌리면서 해결하는건데 넘모 깔끔하당..