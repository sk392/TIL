### All Possible Full Binary Trees



요번엔 [All Possible Full Binary Trees](https://leetcode.com/problems/all-possible-full-binary-trees/)요거를 풀어봐씁니다.

### Problem

A full binary tree is a binary tree where each node has exactly 0 or 2 children.

Return a list of all possible full binary trees with N nodes.  Each element of the answer is the root node of one possible tree.

Each node of each tree in the answer must have node.val = 0.

You may return the final list of trees in any order.

 
```
Example 1:

Input: 7
Output: [[0,0,0,null,null,0,0,null,null,0,0],[0,0,0,null,null,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,null,null,null,null,0,0],[0,0,0,0,0,null,null,0,0]]
Explanation:

```
![image](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/22/fivetrees.png)

 
```
Note:

1 <= N <= 20
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
   fun allPossibleFBT(N: Int): List<TreeNode?> {
       return helper(0, N-1)
    }
	
    val map = HashMap<Int, List<TreeNode>>()
    
    fun helper(start: Int, end: Int) : List<TreeNode> {
        if(map[end - start] != null) return map[end - start]!!
        
        val list = ArrayList<TreeNode>()
        
        if(start == end) {
            list.add(TreeNode(0))
            return list
        }
        
        for(i in (start+1)..(end-1)) {
            val lefts = helper(start, i-1)
            val rights = helper(i + 1, end)
            for(j in 0 until lefts.size) {
                for(k in 0 until rights.size) {
                    val tree = TreeNode(0)
                    tree.left = lefts[j]
                    tree.right = rights[k]
                    list.add(tree)
                }
            }
        }
        map[end - start] = list
        return list
    }
}
```

### Explanation

이것도 1시간 내로 풀지 못해서 디스커스를 봤다.. 하나의 루트 노드를 DP형태로 만드는게 아니라 전체 결과 리스트를 DP로 초점을 맞춰야 된다