###  Maximize Sum Of Array After K Negations


요번엔 [Maximize Sum Of Array After K Negations](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/)요거를 풀어봐씁니다.

### Problem
Given an array A of integers, we must modify the array in the following way: we choose an i and replace A[i] with -A[i], and we repeat this process K times in total.  (We may choose the same index i multiple times.)

Return the largest possible sum of the array after modifying it in this way.

 
```
Example 1:

Input: A = [4,2,3], K = 1
Output: 5
Explanation: Choose indices (1,) and A becomes [4,-2,3].
```

```
Example 2:

Input: A = [3,-1,0,2], K = 3
Output: 6
Explanation: Choose indices (1, 2, 2) and A becomes [3,1,0,2].
```

```
Example 3:

Input: A = [2,-3,-1,5,-4], K = 2
Output: 13
Explanation: Choose indices (1, 4) and A becomes [2,3,-1,5,4].
```

```
Note:

1 <= A.length <= 10000
1 <= K <= 10000
-100 <= A[i] <= 100
```

### Solution

```
class Solution {
    fun largestSumAfterKNegations(A: IntArray, K: Int): Int {
        var result = A.sum()

        for (i in 1..K) {
            val index = A.indexOf(A.min()!!)
            A[index] *= -1
            result += A[index] * 2
        }

        return result
    }
}
```

### Explanation

반전해서 최대의 합을 구하는 문제니까 음수가 있을 땐 가장 작은(절대값이큰) 값을 반전시켜주고, 양수만 있을 땐 양수의 가장 작은값을 반전시켜주면 되는데, 잘 보면 결국엔 젤 낮은 값만 반전시켜주면 된다 ㅇㅈ? ㅇㅇㅈ