### Prime Number of Set Bits in Binary Representation



요번엔 [Prime Number of Set Bits in Binary Representation](https://leetcode.com/problems/prime-number-of-set-bits-in-binary-representation/)요거를 풀어봐씁니다.

### Problem

Given two integers L and R, find the count of numbers in the range [L, R] (inclusive) having a prime number of set bits in their binary representation.

(Recall that the number of set bits an integer has is the number of 1s present when written in binary. For example, 21 written in binary is 10101 which has 3 set bits. Also, 1 is not a prime.)

Example 1:

```
Input: L = 6, R = 10
Output: 4
Explanation:
6 -> 110 (2 set bits, 2 is prime)
7 -> 111 (3 set bits, 3 is prime)
9 -> 1001 (2 set bits , 2 is prime)
10->1010 (2 set bits , 2 is prime)
```

Example 2:

```
Input: L = 10, R = 15
Output: 5
Explanation:
10 -> 1010 (2 set bits, 2 is prime)
11 -> 1011 (3 set bits, 3 is prime)
12 -> 1100 (2 set bits, 2 is prime)
13 -> 1101 (3 set bits, 3 is prime)
14 -> 1110 (3 set bits, 3 is prime)
15 -> 1111 (4 set bits, 4 is not prime)
```

Note:

```
L, R will be integers L <= R in the range [1, 10^6].
R - L will be at most 10000.
```

### Solution

```
class Solution {
    fun countPrimeSetBits(L: Int, R: Int): Int {
        var result = 0
        for(i in L..R){
            if(isPrimeNumber(sumFromConvertedBinary(i))) result++
        }
        return result
    }


    fun sumFromConvertedBinary(value : Int) : Int{
        var result = 0
        value.toString(2).toCharArray().forEach {
            result += it.toString().toInt()
        }
        return result
    }


    fun isPrimeNumber(number : Int) :Boolean{
       if (number <=1) return false
         for(i in 2..Math.sqrt(number.toDouble()).toInt()){
            if(number%i == 0)return false
        }
        return true
    }
}
```

### Explanation

최적화는.. 조금 나중에 생각하더라도 일단 문제의 흐름대로 따라가서 먼저 2진수로 바꾸고 그 값을 더하고 그 값이 프라임넘버인지 확인하는 코드로 짰다