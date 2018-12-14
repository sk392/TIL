### addTwoNumbers


요번엔 [addTwoNumbers](https://leetcode.com/problems/add-two-numbers/)요거를 풀어봐씁니다.

### Problem

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example:

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

### Solution

```

/**
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int = 0) {
 *     var next: ListNode? = null
 * }
 */
class Solution {
    fun addTwoNumbers(l1: ListNode?, l2: ListNode?): ListNode? {        

    var currentNode1 = l1
    var currentNode2 = l2

    var isCarry = false
    val result = ListNode(0)
    var temp: ListNode? = result
    while (true) {
        
        var sum = ((currentNode1?.`val` ?: 0) + (currentNode2?.`val` ?: 0))
        if (isCarry) {
            sum++
            isCarry = false
        }

        if (sum >= 10) {
            isCarry = true
            sum %= 10
        }
        print("$sum");
        temp?.`val` = sum

        currentNode1 = currentNode1?.next
        currentNode2 = currentNode2?.next

        if(currentNode1 != null || currentNode2 != null || isCarry){
            temp?.next = ListNode(0)
            temp = temp?.next
        }else{
            return result
        }
    }

    return result
       }  

}

```

### Description

않이... 열심히 고민해서 와일문 하나에 넣어놨더니; 개오래걸린다... 분하다... 하지만 피곤하다.. 이 분함은 내일의 나에게 넘긴다!

```
Runtime: 300 ms, faster than 7.89% of Kotlin online submissions for Add Two Numbers.
```
