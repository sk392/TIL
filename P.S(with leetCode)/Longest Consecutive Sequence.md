### Longest Consecutive Sequence



Today's Problem is Hard - [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

### Problem

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

Your algorithm should run in O(n) complexity.

Example:

```
Input: [100, 4, 200, 1, 3, 2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

### Solution

```
class Solution {
    fun longestConsecutive(nums: IntArray): Int {
        if(nums.isEmpty()) return 0
        
        val set = hashSetOf<Int>()
        var max = Integer.MIN_VALUE
        
        nums.forEach {
            set.add(it)
        }
        
        set.forEach { num ->
            if(!set.contains(num - 1)) {
                var current = num
                var streak = 1
                
                while(set.contains(current + 1)) {
                    streak += 1
                    current += 1
                }
                
                max = Math.max(max, streak)
            }
        }
        
        return max
    }
}
```

### Explanation

음.. 우선 set을 사용해서 모아서 양옆에 연결된 친구가 있으면 양 옆에 연결된 값들을 계산해서 양옆값도 갱신해주고 이런식으로 진행했는데, 빵꾸가 많아서 로직이 점점 더러워져갔다.
 결국엔 못풀어보고 dicuss에서 힌트를 보고 진행했는데, 굉장하다! 이런게 똑똑한게 아닐까 싶다!
 자기 왼쪽에 값이 없다면(시작위치라면) 오른쪽으로 계속 더해가면서 맥스값을 갱신시켜준다.. 이거 진짜 놀랍다!! 똑똑쓰