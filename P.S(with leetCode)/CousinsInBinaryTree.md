### CousinsInBinaryTree


요번엔 [CousinsInBinaryTree](https://leetcode.com/problems/cousins-in-binary-tree/)요거를 풀어봐씁니다.

### Problem

In a binary tree, the root node is at depth 0, and children of each depth k node are at depth k+1.

Two nodes of a binary tree are cousins if they have the same depth, but have different parents.

We are given the root of a binary tree with unique values, and the values x and y of two different nodes in the tree.

Return true if and only if the nodes corresponding to the values x and y are cousins.

 
```
Example 1:


Input: root = [1,2,3,4], x = 4, y = 3
Output: false
```

```
Example 2:
Input: root = [1,2,3,null,4,null,5], x = 5, y = 4
Output: true
```

```
Example 3:


Input: root = [1,2,3,null,4], x = 2, y = 3
Output: false
 ```

Note:

The number of nodes in the tree will be between 2 and 100.
Each node has a unique integer value from 1 to 100.

### Solution

```

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
    fun isCousins(root: TreeNode?, x: Int, y: Int) :Boolean{
        root ?: return false
        val nodes = mutableListOf<TreeNode>()
        nodes.add(root)

        while(nodes.isNotEmpty()){
            val tempNodes = mutableListOf<TreeNode>()
            var findX = false
            var findY = false
            nodes.forEach{ node ->
                findX = findX || node.`val` == x
                findY = findY || node.`val` == y

                val leftValue = node.left?.also{tempNodes.add(it)}?.`val` ?: 0
                val rightValue =  node.right?.also{tempNodes.add(it)}?.`val` ?: 0

                if((leftValue == x || leftValue == y) && (rightValue == x || rightValue == y)) return false
            }
            if(findX && findY) return true
            nodes.clear()
            nodes.addAll(tempNodes)
        }
        return false
    }
}

```

### Explanation

생각한대로 풀었는데 왜 faster then 100%지..? 이러면안되는데;;
