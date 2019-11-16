### Merge k Sorted Lists


오늘의 문제는 HARD- [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

### Problem

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

Example:

```
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```


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
    fun mergeKLists(lists: Array<ListNode?>): ListNode? {
         if(lists.isEmpty()) return null
       val headers = MutableList(lists.size) { lists[it] }
        var result: ListNode? = null
        var temp: ListNode? = null

        do {
            val index = getMinimumNodeIndex(headers)

            if(result == null){
                result = headers[index]
                temp = result
            }else{
                temp?.next = headers[index]
                temp = temp?.next
            }
            headers[index] = headers[index]?.next
        }while (temp != null)

        return result
    }

    fun getMinimumNodeIndex(headers: MutableList<ListNode?>): Int {
        var minimumValue = Int.MAX_VALUE
        var minimumIndex = 0

        for (i in 0 until headers.size) {
            val value = headers[i]?.`val` ?: Int.MAX_VALUE
            if (minimumValue > value) {
                minimumValue = value
                minimumIndex = i
            }
        }
        return minimumIndex
    }
}
```

### Explanation

띠용..? Mideum인줄 알았는데 난이도가 Hard라니..? 게다가 emptyList가 들어오는 경우 에대한 예외처리를 안한거 빼고는 한방에 풀었다.. 어리둥절..ㅋㅋㅋ

 무튼 문제는 각 ListNode별로 헤더를 가지는 노드리스트를 하나 가진다. 그리고 그 리스트에서 최저 값을 구한다음에 가지고 있는 curruentNode를 계속해서 갱신해준다. 문제가 왜 하드일까..?