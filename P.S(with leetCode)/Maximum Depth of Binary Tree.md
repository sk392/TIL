### Maximum Depth of Binary Tree

Today's Problem is EASY - [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

### Problem
Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Note: A leaf is a node with no children.

Example:

```
Given binary tree [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
return its depth = 3.
```

### Solution

```
import java.util.*
class Solution {
    fun maxDepth(root: TreeNode?): Int {
        root ?: return 0
        val queue : Queue<Pair<TreeNode,Int>> = LinkedList()
        var depth = 0
        queue.add(Pair(root,0))

        while(queue.isNotEmpty()){
            val curr = queue.poll()
            val node = curr.first
            val currDepth = curr.second
            if(node.left != null){
                queue.add(Pair(node.left!!,currDepth+1))
            }
            if(node.right != null){
                queue.add(Pair(node.right!!,currDepth+1))
            }
            depth = currDepth
        }

        return depth +1
    }
}
```

### Explanation

바로 이전에 level order traversal 을 통해서 해결했던 것과 같은 방법으로 해결!