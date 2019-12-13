## Subsets



Today's Problem Medium - [Subsets](https://leetcode.com/problems/subsets/)

### Problem

Given a set of distinct integers, nums, return all possible subsets (the power set).

Note: The solution set must not contain duplicate subsets.

Example:

```Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

### Solution

```
//solution1

class Solution {
    fun subsets(nums: IntArray): List<List<Int>> {
        val result = mutableListOf<List<Int>>()
        result.add(listOf())
        for(i in 0 until nums.size){
            val value = nums[i]
            for(item in result.toList()){
                val list = item.toMutableList()
                list.add(value)
                result.add(list)
            }
        }
        return result
    }
}
```

```
//solution2

class Solution {
    fun subsets(nums: IntArray): List<List<Int>> {
        val result = mutableListOf<List<Int>>()
        addSubSets(nums, mutableListOf(), 0, result)
        result.add(listOf())

        return result
    }

    fun addSubSets(nums: IntArray, list: MutableList<Int>, startIndex: Int, result: MutableList<List<Int>>) {
        for (i in startIndex until nums.size) {
            val value = nums[i]
            if (!list.contains(value)) {
                val mutableList = list.toMutableList()
                mutableList.add(value)
                result.add(mutableList)
                addSubSets(nums, mutableList, i, result)
            }
        }
    }


}

```

### Explanation

처음엔 solution2방법으로 [Permutations](https://github.com/sk392/TIL/blob/master/P.S(with%20leetCode)/Permutations.md)에서 참고해서 풀었다가 좀 더 간단하게 풀 수 있을 것같아서 첫번째 처럼 풀었다.

 처음에 result에 emptyList를 넣고 result에 있는 리스트를 하나씩 뽑아서 숫자를 넣어준다 그러면 
 [] ->
 [],[1] ->
 [],[1],[1,2],[2] ->
 [],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]
 
 이런식으로 들어간다 음.. 다시 생각해보니 2번 방식이 더 좋은 것같다. 코드는 1번이 간단한뎅..
 
 1번로직은 1,2,4.. 처럼 2의 배수로 늘어나고, 2번 로직은 n제곱이다. n이 큰 숫자일수록 첫번째 로직의 성능은 기하급수 적으로 떨어진다. 참고로 등비 수열의 합의 공식은 다음과 같다
 
 ![](https://t1.daumcdn.net/cfile/tistory/22520A365365333507)