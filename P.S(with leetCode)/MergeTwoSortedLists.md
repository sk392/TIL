###  Merge Two Sorted Lists


요번엔 [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)요거를 풀어봐씁니다.

### Problem
Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

Example:

```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
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
    fun mergeTwoLists(l1: ListNode?, l2: ListNode?): ListNode? {
    if(l1== null && l2 == null){
        return null
    }
        val result = ListNode(0)
    var tempNode = result
    var temp1 = l1
    var temp2 = l2
    while (temp1 != null || temp2 != null) {
        if (temp1 == null) {
            tempNode.`val` = temp2?.`val`!!
            temp2 = temp2.next
        } else if (temp2 == null) {
            tempNode.`val` = temp1.`val`
            temp1 = temp1.next
        } else {
            if (temp1.`val` > temp2.`val`) {
                tempNode.`val` = temp2.`val`
                temp2 = temp2.next
            } else {
                tempNode.`val` = temp1.`val`
                temp1 = temp1.next
            }
        }
        if(temp1 != null || temp2 != null){

        tempNode.next = ListNode(0)
        tempNode = tempNode.next!!
        }
    }

    return result
    }
}
```

### Explanation

와.. 생각나는데로 풀었더니 이렇게 나오네 ㅋㅋㅋㅋㅋㅋ 개선해봐야게따