### Sort List



Today's Problem is Medium - [Sort List](https://leetcode.com/problems/sort-list/)

### Problem


Sort a linked list in O(n log n) time using constant space complexity.

Example 1:

```
Input: 4->2->1->3
Output: 1->2->3->4
```

Example 2:

```
Input: -1->5->3->4->0
Output: -1->0->3->4->5
```


### Solution

```kotlin
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
    fun sortList(head: ListNode?): ListNode? {
        if(head?.next == null) return head

        var slow= head
        var fast= head
        var pre :ListNode? = null

        while(fast?.next !=null){
            pre = slow
            slow = slow?.next
            fast = fast.next?.next
        }
        pre?.next = null

        val mid = slow
        val left = sortList(head)
        val right = sortList(mid)

        return mergeSort(left,right)
    }

    fun mergeSort(left : ListNode?, right : ListNode?) : ListNode?{
        var current = ListNode()
        val dummy = current
        var left = left
        var right= right

        while(left != null && right !=null){
            if(left.`val` < right.`val`){
                current.next = left
                left = left.next
            }else{
                current.next = right
                right = right.next
            }
            current = current.next!!
        }
        if(left != null){
            current.next = left
        }else if( right!=null){
            current.next = right
        }

        return dummy.next
    }
}
```

### Explanation

n log n으로 해결하기위해서 최초의 mergeSort를 생각했는데  생각보다 까다로워서 고생헀다. 첫번째로 머지소트를 위해선 리스트를 반으로 갈라서 해야하는 부분이있고, 두번째는 정렬할 때 인덱스를 사용할 수 없고 node의 next를 지정함으로써 정렬이 되어야하고. 그렇기 때문에 기존에 있는 노드를 단순히 연결하는 것 뿐만아니라 핻아 노드의 next도 끊어줘야 한다. 
 오랜만에 풀어서 그런지 까다로운 문제였다!