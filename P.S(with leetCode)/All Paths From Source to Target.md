###  All Paths From Source to Target


요번엔 [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)요거를 풀어봐씁니다.

### Problem

The graph is given as follows:  the nodes are 0, 1, ..., graph.length - 1.  graph[i] is a list of all nodes j for which the edge (i, j) exists.

```
Example:
Input: [[1,2], [3], [3], []]
Output: [[0,1,3],[0,2,3]]
Explanation: The graph looks like this:
0--->1
|    |
v    v
2--->3
There are two paths: 0 -> 1 -> 3 and 0 -> 2 -> 3.
```

```
Note:

The number of nodes in the graph will be in the range [2, 15].
You can print different paths in any order, but you should keep the order of nodes inside one path.
```

### Solution

```
class Solution {
    val res = ArrayList<List<Int>>()

    fun allPathsSourceTarget(graph: Array<IntArray>): List<List<Int>> {
        val path = ArrayList<Int>()

        path.add(0)
        dfsSearch(graph, 0, path)

        return res
    }

    private fun dfsSearch(graph: Array<IntArray>, node: Int, path: MutableList<Int>) {
        if (node == graph.size - 1) {
            res.add(ArrayList(path))
            return
        }

        for (nextNode in graph[node]) {
            path.add(nextNode)
            dfsSearch(graph, nextNode, path)
            path.removeAt(path.size - 1)
        }
    }
}

```

### Explanation

모든 길 찾기 문제이당. DFS로 해결!