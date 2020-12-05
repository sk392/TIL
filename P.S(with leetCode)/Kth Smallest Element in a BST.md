### Kth Smallest Element in a BST



Today's Problem is Medium - [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

### Problem

Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.

 

Example 1:

```
Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1
```

Example 2:

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
```

Follow up:

```
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?
```

### Solution

```
class Solution {
    private Integer result = null;

    public int kthSmallest(TreeNode root, int k) {
        getChildCount(root, k, 0);
        return result == null ? 0 : result;
    }

    private int getChildCount(TreeNode root, int k, int index) {
        if (root == null || result != null) return 0;
        int leftCount = getChildCount(root.left, k, index);
        index++;
        if (index + leftCount == k) {
            result = root.val;
        }

        int rightCount = getChildCount(root.right, k, leftCount + index);

        return leftCount + rightCount + 1;
    }
}
```

### Explanation

바이너리 서치의 특징을 우선 잘 이해해야한다.

루트의 left 차일드는 무조건 루트보다 낮고, right는 무조건 루트보다 크다.

그래서 몇번 째로 작은 숫자를 구하려면 가장 왼쪽노드부터 찾아야한다.

키는 2가지다 나를 포함한 차일드의 카운트는 left + right + 1이고, 루트를 체크할 때마다 index를 늘려간다. index가 레퍼런스 변수여서는 안된다.
