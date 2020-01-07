### Binary Tree Level Order Traversal



Today's Problem is Medium - [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

### Problem

Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:

```
Given binary tree [3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7
```

return its level order traversal as:

```
[
  [3],
  [9,20],
  [15,7]
]
```

### Solution

```
import java.util.*
/**
 * Example:
 * var ti = TreeNode(5)
 * var v = ti.`val`
 * Definition for a binary tree node.
 * class TreeNode(var `val`: Int) {
 *     var left: TreeNode? = null
 *     var right: TreeNode? = null
 * }
 */
class Solution {
    fun levelOrder(root: TreeNode?): List<List<Int>> {
        root ?: return emptyList()
        val result = mutableListOf<List<Int>>()
        val queue : Queue<Pair<TreeNode,Int>> =ArrayDeque()

        queue.add(Pair(root,0))
        var currentLevel = 0
        var list = mutableListOf<Int>()
        while(queue.isNotEmpty()){
            val current = queue.poll()
            val node = current.first
            val level = current.second

            if(node.left != null){
                queue.add(Pair(node.left!!,level+1))
            }
            if(node.right != null){
                queue.add(Pair(node.right!!,level+1))
            }

            if(currentLevel == level){
                list.add(node.`val`)
            }else{
                result.add(list.toList())
                list = mutableListOf()
                list.add(node.`val`)
                currentLevel++
            }
        }

        result.add(list.toList())

        return result
    }
}
```

### Explanation

레벨 순회방법은 큐를 사용해서 알고리즘을 풀어나가는데 조금 생각해봐야 할 것은 list를 어떤 단위로 끊을 것인지에 대해서 고민해봐야한다. 때문에 Queue에 Pair를 넣어서 레벨도 함께 넣을 수 있도록 만들어주고 현재 level이 몇인지 저장해 뒀다가, node의 레벨이 바뀌면 새로운 리스트를 만들어주고 마지막에 저장되지 않은 리스트가 있을테니 저장해준다.