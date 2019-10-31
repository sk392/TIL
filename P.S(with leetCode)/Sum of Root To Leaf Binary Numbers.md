###  Sum of Root To Leaf Binary Numbers


요번엔 [Sum of Root To Leaf Binary Numbers](https://leetcode.com/problems/sum-of-root-to-leaf-binary-numbers/)요거를 풀어봐씁니다.

### Problem

Given a binary tree, each node has value 0 or 1.  Each root-to-leaf path represents a binary number starting with the most significant bit.  For example, if the path is 0 -> 1 -> 1 -> 0 -> 1, then this could represent 01101 in binary, which is 13.

For all leaves in the tree, consider the numbers represented by the path from the root to that leaf.

Return the sum of these numbers.

 

Example 1:
![](https://assets.leetcode.com/uploads/2019/04/04/sum-of-root-to-leaf-binary-numbers.png)


```
Input: [1,0,1,0,1,0,1]
Output: 22
Explanation: (100) + (101) + (110) + (111) = 4 + 5 + 6 + 7 = 22
```
 

Note:

```
The number of nodes in the tree is between 1 and 1000.
node.val is 0 or 1.
The answer will not exceed 2^31 - 1.
```

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
    var result = 0

    fun sumRootToLeaf(root: TreeNode?): Int {
        getResultRootToLeaf(root, "")
        return result
    }

    fun getResultRootToLeaf(node: TreeNode?, tempResult: String) {
        node ?: return
        val sumResult = tempResult + node.`val`
        if (!isHaveChild(node)) {
            result += sumResult.toInt(2)
        } else {
            getResultRootToLeaf(node.left, sumResult)
            getResultRootToLeaf(node.right, sumResult)
        }
    }

    fun isHaveChild(node: TreeNode?) = (node?.left != null || node?.right != null)
}

```

### Explanation

DFS에 가장 기본적인 문제인 것같다. 순차적으로 노드를 들어가면서 끝 노드에 도착하면 값을 더해주면 되는 간단한 문제!