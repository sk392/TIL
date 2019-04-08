### DistributeCoinsinBinaryTree


요번엔 [DistributeCoinsinBinaryTree](https://leetcode.com/problems/distribute-coins-in-binary-tree/)요거를 풀어봐씁니다.

### Problem

Given the root of a binary tree with N nodes, each node in the tree has node.val coins, and there are N coins total.

In one move, we may choose two adjacent nodes and move one coin from one node to another.  (The move may be from parent to child, or from child to parent.)

Return the number of moves required to make every node have exactly one coin.


```
Example 1:



Input: [3,0,0]
Output: 2
Explanation: From the root of the tree, we move one coin to its left child, and one coin to its right child.

```

```
Example 2:



Input: [0,3,0]
Output: 3
Explanation: From the left child of the root, we move two coins to the root [taking two moves].  Then, we move one coin from the root of the tree to the right child.
```


```
Example 3:



Input: [1,0,2]
Output: 2
```

```
Example 4:



Input: [1,0,0,null,3]
Output: 4
```

```
Note:

1<= N <= 100
0 <= node.val <= N
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
    var distributeCoinsResult = 0

    fun distributeCoins(root: TreeNode?): Int {
    if(root != null){
        distributeCoins(root.left)
        distributeCoins(root.right)

        root.left?.let{
            val value =  it.`val` -1
            root.`val` += value
            distributeCoinsResult += Math.abs(value)
            it.`val`=1
        }

        root.right?.let{
            val value =  it.`val` -1
            root.`val` += value
            distributeCoinsResult += Math.abs(value)
            it.`val`=1
        }
    }
    return distributeCoinsResult  
    }
}

```

### Explanation

왠지 DP방식으로 많이 고민하게 됐는데.. 이거 좀 헷갈리는데 DFS인가..? 헷갈림...
