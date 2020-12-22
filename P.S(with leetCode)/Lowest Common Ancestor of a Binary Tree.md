### Lowest Common Ancestor of a Binary Tree



Today's Problem is Medium - [Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/palindrome-linked-list/)

### Problem


Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

 

Example 1:

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

Example 2:


```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

Example 3:

```
Input: root = [1,2], p = 1, q = 2
Output: 1
``` 

Constraints:

```
The number of nodes in the tree is in the range [2, 105].
-109 <= Node.val <= 109
All Node.val are unique.
p != q
p and q will exist in the tree.
```


### Solution

```kotlin

class Solution {
    fun lowestCommonAncestor(root: TreeNode?, p: TreeNode?, q: TreeNode?): TreeNode? {
        val pAncestorList = mutableListOf<TreeNode?>()
        val qAncestorList = mutableListOf<TreeNode?>()
        var lowestCommonAncestor: TreeNode? = root

        findAncestor(root, p, pAncestorList)
        findAncestor(root, q, qAncestorList)

        for (i in 0 until Math.min(pAncestorList.size, qAncestorList.size)) {
            if (pAncestorList[i] != qAncestorList[i]) break

            lowestCommonAncestor = pAncestorList[i]
        }

        return lowestCommonAncestor
    }

    private fun findAncestor(root: TreeNode?, p: TreeNode?, list: MutableList<TreeNode?>): Boolean {
        root ?: return false

        list.add(root)
        if (p != root) {
            val result = findAncestor(root.right, p, list) || findAncestor(root.left, p, list)
            if (!result) list.remove(root)
            return result
        }
        return true
    }
}
```

### Explanation

미디엄문제는 역시 풀만한 것같다.
각각 p,q를 찾는 재귀를 만들고 도달하면 현재까지 지나왔던 노드들을 저장하는 리스트를 들고 있는다.
그렇게 각각 노드들을 저장한 리스트를 0번부터 차례로 다른것을 찾을 때까지 포문을 돌아서 리턴해주면 해결!.

재귀할 땐 찾았을 때 더이상 해당 값에 대한 재귀가 발생하지 않도록 할 수 있도록 return값을 Boolean으로 반환하도록 하고, ||연산의 특징을 활용해 찾은경우 다른 재귀가 발생하지 않도록 제한을 걸었고, 레퍼런스로 변수가 넘어가고 있기 때문에 해당 루트에서 찾지 못헀다면 해당 리스트에서 현재 루트를 제거해줘야한다.