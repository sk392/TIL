###  Lemonade Change


요번엔 [Lemonade Change](https://leetcode.com/problems/lemonade-change/)요거를 풀어봐씁니다.

### Problem

At a lemonade stand, each lemonade costs $5. 

Customers are standing in a queue to buy from you, and order one at a time (in the order specified by bills).

Each customer will only buy one lemonade and pay with either a $5, $10, or $20 bill.  You must provide the correct change to each customer, so that the net transaction is that the customer pays $5.

Note that you don't have any change in hand at first.

Return true if and only if you can provide every customer with correct change.

 
```
Example 1:

Input: [5,5,5,10,20]
Output: true
Explanation: 
From the first 3 customers, we collect three $5 bills in order.
From the fourth customer, we collect a $10 bill and give back a $5.
From the fifth customer, we give a $10 bill and a $5 bill.
Since all customers got correct change, we output true.
```

```
Example 2:

Input: [5,5,10]
Output: true
```

```
Example 3:

Input: [10,10]
Output: false
```

```
Example 4:

Input: [5,5,10,10,20]
Output: false
Explanation: 
From the first two customers in order, we collect two $5 bills.
For the next two customers in order, we collect a $10 bill and give back a $5 bill.
For the last customer, we can't give change of $15 back because we only have two $10 bills.
Since not every customer received correct change, the answer is false.
 
```


```
Note:

0 <= bills.length <= 10000
bills[i] will be either 5, 10, or 20.
```

### Solution

```
class Solution {
    fun lemonadeChange(bills: IntArray): Boolean {
        val casher = mutableMapOf<Int, Int>()

        for (i in bills) {
            when (i) {
                5 -> {
                    casher[5] = (casher[5] ?: 0) + 1
                }
                10 -> {
                    if (casher[5] == null || casher[5] == 0) return false
                    casher[5] = (casher[5] ?: 0) - 1
                    casher[10] = (casher[10] ?: 0) + 1
                }
                20 -> {
                    if (casher[5] == null || casher[5] == 0) return false
                    if(casher[10] == null || casher[10] == 0){
                        if(casher[5] ?:0 < 3) return false
                        casher[5] = (casher[5] ?: 0) - 3
                    }else{
                        casher[5] = (casher[5] ?: 0) - 1
                        casher[10] = (casher[10] ?: 0) - 1
                    }
                }
            }
        }
        return true

    }
}
```

### Explanation

오 쉬운 것에 비해서 조금신경써야할 것들이 있어서 몇번 틀렸는데 그럭저럭 괜찮은 문제인듯?? 코테 1번정도 수준의..?