### Merge Intervals


Today's Problem Medium - [Merge Intervals](https://leetcode.com/problems/merge-intervals/)

### Problem

Given a collection of intervals, merge all overlapping intervals.

Example 1:

```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

Example 2:

```
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

NOTE: input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

### Solution

```
class Solution {
    fun merge(intervals: Array<IntArray>): Array<IntArray> {
        if (intervals.size in 0..1) return intervals
        
         intervals.sortBy { ints -> ints[0] }

        val result = mutableListOf<IntArray>()
        var array = mutableListOf<Int>()
        var maximumIndex = 0

        for (i in 0 until intervals.size) {
            val current = intervals[i]
            if (array.isEmpty()) {
                array.add(current[0])
            } else {
                if (current[0] > maximumIndex) {
                    array.add(maximumIndex)
                    result.add(array.toIntArray())
                    array = mutableListOf()
                    array.add(current[0])
                }
            }

            maximumIndex = Math.max(maximumIndex,current[1])

            if(i == intervals.size-1){
                array.add(maximumIndex)
                result.add(array.toIntArray())
            }
        }
        return result.toTypedArray()
    }
}
```

### Explanation

먼저 생각해볼 것은 정렬되어 있는지, 범위가 0일수도 있는지 체크해본다.

for문을 하나씩 돌면서 왼쪽이 작은 값 오른쪽이 큰 값인데, 이전 항의 오른쪽 항중에 최고값(maximumIndex)이 현재 항의 왼쪽보다 크다면 포함 관계로 넣어주고 최고값보다 왼쪽 항이 크다면 이전까지 어레이를 넣어주고 새로 어레이를 발급한다.

요기서 중요한건 위에도 적어놨듯이 왼쪽 항 기준으로 정렬되어 있어야하고, 범위가 0일수도 있다. 정렬이 가장 중요하다! 때문에 복잡도는  O(nlogn)이 되었지만.. 엣지 케이스를 전부 처리하기가 까다로울 수 있다.