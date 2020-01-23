### Construct Binary Tree from Preorder and Inorder Traversal



Today's Problem is Medium - [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)


### Problem


Given preorder and inorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

For example, given

```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
   
```

### Solution

```
class Solution {
    private var preIdx = 0
    private lateinit var preorder: IntArray
    private lateinit var inorder: IntArray
    private lateinit var inorderMap: MutableMap<Int, Int>

    fun buildTree(preorder: IntArray, inorder: IntArray): TreeNode? {
        this.preIdx = 0
        this.inorderMap = mutableMapOf()
        this.inorder = inorder
        this.preorder = preorder

        for (i in inorder.indices) {
            inorderMap[inorder[i]] = i
        }

        return constructTree(0, inorder.size)
    }

    private fun constructTree(
        inLeft: Int, // inclusive
        inRight: Int // exclusive
    ): TreeNode? {
        if (inLeft == inRight) return null

        val rootVal = preorder[preIdx]
        val rootIdx = inorderMap[rootVal]!!
        val root = TreeNode(rootVal)

        preIdx++
        root.left = constructTree(inLeft, rootIdx)
        root.right = constructTree(rootIdx + 1, inRight)

        return root
    }
}
```

### Explanation

savethemonkey에서 inorder방식과 postorder방식으로 트리 구조 찾는 문제를 풀었었는데 여긴 조금 다르게 inorder와 preorder를 사용해서 문제를 풀었다.

최초엔 리스트를 슬라이스하는 방법으로 했지만, 좋은 솔루션을 찾아내었다.
여기서 가장 중요한 변수는 preIdx인데, preorder방식으로 리커시브하게 호출될 때마다 한칸씩 늘어가는데, 아주 환상적이당.