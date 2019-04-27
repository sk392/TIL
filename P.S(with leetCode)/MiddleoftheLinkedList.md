###  MiddleoftheLinkedList


요번엔 [MiddleoftheLinkedList](https://leetcode.com/problems/middle-of-the-linked-list/)요거를 풀어봐씁니다.

### Problem

Given a non-empty, singly linked list with head node head, return a middle node of linked list.

If there are two middle nodes, return the second middle node.

 

Example 1:

Input: [1,2,3,4,5]
Output: Node 3 from this list (Serialization: [3,4,5])
The returned node has value 3.  (The judge's serialization of this node is [3,4,5]).
Note that we returned a ListNode object ans, such that:
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, and ans.next.next.next = NULL.
Example 2:

Input: [1,2,3,4,5,6]
Output: Node 4 from this list (Serialization: [4,5,6])
Since the list has two middle nodes with values 3 and 4, we return the second one.
 

Note:

The number of nodes in the given list will be between 1 and 100.
### Solution

```
/**
 * Example:
 * var li = ListNode(5)
 * var v = li.`val`
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int) {
 *     var next: ListNode? = null
 * }
 */
class Solution {
    fun middleNode(head: ListNode?): ListNode? {
                var header = head
        var result = head
        var cnt = 1
        while(header?.next != null){
            cnt++
            header = header.next
            if(cnt%2==0){
                result = result?.next
            }
        }
        return result
    }
}
```

### Explanation

2칸마다 1칸 전진해주는 해드만 찾으면 댄다! 간단잼