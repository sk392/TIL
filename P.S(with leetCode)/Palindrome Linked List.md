### Palindrome Linked List



Today's Problem is Easy - [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

### Problem


Given a singly linked list, determine if it is a palindrome.

Example 1:

```
Input: 1->2
Output: false
```

Example 2:

```
Input: 1->2->2->1
Output: true
```

Follow up:

```
Could you do it in O(n) time and O(1) space?
```


### Solution

```kotlin

class Solution {
    fun isPalindrome(head: ListNode?): Boolean {
        var slow = head
        var fast = head

        while (fast != null){
            slow = slow?.next 
            fast = fast.next?.next 
        }

        slow = reverse(slow)
        fast = head

        while (slow != null) {
            if (fast?.`val` != slow.`val`) {
                return false
            }
            fast = fast.next
            slow = slow.next
        }
        return true;
    }

    fun reverse(head: ListNode?): ListNode? {
        var newHead = head
        var prev: ListNode? = null
        while (newHead != null) {
            val next = newHead.next
            newHead.next = prev
            prev = newHead
            newHead = next
        }
        return prev
    }
}
```

### Explanation

문제 자체를 푸는 것은 어렵지 않으나 O(1)로 풀 수 있는 방법은 꽤나 어렵다. 문제는 easy로 되어있지만 O(1)로 풀거면 Medium정도는 되는 것같다.

처음엔 Sum으로 체크해서 반복이 시작되는 부분을 찾아서 그 때부터는 -로 값을 빼는 방법으로 계산했었는데 이 경우 값 자체는 같지만 순서가 다른경우에 대한 체크는 할 수 없다.

slow와 fast를 구분해서 slow를 절반 위치까지 보내고 그 다음에 리버스 하고, 값이 같은지 대조하는 것이다.

어썸!