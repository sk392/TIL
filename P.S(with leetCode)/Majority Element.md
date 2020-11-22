### Majority Element



Today's Problem is Easy - [Majority Element](https://leetcode.com/problems/majority-element/)

### Problem

Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

You may assume that the array is non-empty and the majority element always exist in the array.

Example 1:

```
Input: [3,2,3]
Output: 3
```
Example 2:

```
Input: [2,2,1,1,1,2,2]
Output: 2
```

### Solution

```
fun majorityElement(nums: IntArray): Int {
    var major = nums[0]
    var count = 1
    for (i in 1 until nums.size) {
        when {
            count == 0 -> {
                major = nums[i]
                count++
            }
            nums[i] == major -> {
                count++
            }
            else -> count--
        }
    }
    return major
}
```

### Explanation

먼저 brute force로 다가가서 map에 우선 하나씩 넣고 map에 해당 값의 카운트가 사이즈보다 크면 리턴하는 식으로 전개했는데 그럼 공간복잡도가 최대 N까지 나오게된다. 고민해 공간복잡도가 1로 나오게 하는 로직으로 수정했다.