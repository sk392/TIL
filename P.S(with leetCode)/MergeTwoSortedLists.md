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
        if (l1 == null && l2 == null) {
            return null
        }
        val result = ListNode(0)
        var temp1 = l1
        var temp2 = l2

        var tempNode = result

        while (temp1 != null || temp2 != null) {

            if (temp1?.`val` ?: Int.MAX_VALUE > temp2?.`val` ?: Int.MAX_VALUE) {
                tempNode.next = temp2
                temp2 = temp2?.next
            } else {
                tempNode.next = temp1
                temp1 = temp1?.next
            }

            tempNode = tempNode.next!!
        }

        return result.next
    }
}
```

### Explanation

와.. 생각나는데로 풀었더니 이렇게 나오네 ㅋㅋㅋㅋㅋㅋ 개선해봐야게따

--

엄.. 속도는 일단 개선되긴 했고.. 매번 체크하던 if문을 줄였고.. 초기화가 필요해서 코드는 늘었지만.. 먼가 속도를 위한 개선이니까..;; 실코드는 이런식으로 짜면 안되고 읽게 좋게짜자!

--
와! result.next를 하면 된다.. 간단한건데 이런게 생각이 잘안난다.. 코드 량을 많이 줄였다 고마워요 아리아!