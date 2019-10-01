### Partition Array Into Three Parts With Equal Sum

요번엔 [Partition Array Into Three Parts With Equal Sum](https://leetcode.com/problems/partition-array-into-three-parts-with-equal-sum/)요거를 풀어봐씁니다.

### Problem
Given an array A of integers, return true if and only if we can partition the array into three non-empty parts with equal sums.

Formally, we can partition the array if we can find indexes i+1 < j with (A[0] + A[1] + ... + A[i] == A[i+1] + A[i+2] + ... + A[j-1] == A[j] + A[j-1] + ... + A[A.length - 1])

 

Example 1:

```
Input: [0,2,1,-6,6,-7,9,1,2,0,1]
Output: true
Explanation: 0 + 2 + 1 = -6 + 6 - 7 + 9 + 1 = 2 + 0 + 1
```
Example 2:

```
Input: [0,2,1,-6,6,7,9,-1,2,0,1]
Output: false
```
Example 3:

```
Input: [3,3,6,5,-2,2,5,1,-9,4]
Output: true
Explanation: 3 + 3 = 6 = 5 - 2 + 2 + 5 + 1 - 9 + 4
``` 

Note:

```
3 <= A.length <= 50000
-10000 <= A[i] <= 10000
```

### Solution

```

class Solution {
    fun canThreePartsEqualSum(A: IntArray): Boolean {
        if (A.sum() % 3 != 0) return false
        val sumResult = A.sum() / 3
        var currentSum = 0
        for (i in 0 until A.size) {
            currentSum += A[i]
            if (currentSum == sumResult) {
                currentSum = 0
            }
        }
        return currentSum == 0
    }
}
```

### Explanation

엄청 오랜만에 풀어서.. 감을 다 잃었습니당... 이걸 DP로 풀다가 멘붕와서 디스커스보고 해결..ㅠㅠ 몽총쓰...