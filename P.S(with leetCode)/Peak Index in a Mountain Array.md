### Peak Index in a Mountain Array


요번엔 [Peak Index in a Mountain Array](https://leetcode.com/problems/peak-index-in-a-mountain-array/)요거를 풀어봐씁니다.

### Problem

Let's call an array A a mountain if the following properties hold:

A.length >= 3
There exists some 0 < i < A.length - 1 such that A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]
Given an array that is definitely a mountain, return any i such that A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1].

Example 1:

```
Input: [0,1,0]
Output: 1
```
Example 2:

```
Input: [0,2,1,0]
Output: 1
```
Note:

```
3 <= A.length <= 10000
0 <= A[i] <= 10^6
A is a mountain, as defined above.
```

### Solution

```
class Solution {
    fun peakIndexInMountainArray(A: IntArray): Int {
        var index= 0
        var maxValue = 0
        for ( i in 1 until A.size-1){
            if(A[i-1] < A[i] && A[i] > A[i+1]){
                if(maxValue < A[i]){
                    maxValue = A[i]
                    index = i
                }
            }
        }
        return index
    }
}
```

### Explanation

이건... 알고리즘 문제라고 할 수 있나; 그냥 고민할 것없이 3분만에 풀었다, 랜덤 문제를 찍다보면 가끔 따봉보다 싫어요가 더 많은 문제들이 있는데 이유가 있는 것같다.