### Network Delay Time

요번엔 [Network Delay Time](https://leetcode.com/problems/binary-tree-pruning/)요거를 풀어봐씁니다.

### Problem
There are N network nodes, labelled 1 to N.

Given times, a list of travel times as directed edges times[i] = (u, v, w), where u is the source node, v is the target node, and w is the time it takes for a signal to travel from source to target.

Now, we send a signal from a certain node K. How long will it take for all nodes to receive the signal? If it is impossible, return -1.

 

Example 1:

![](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)

```
Input: times = [[2,1,1],[2,3,1],[3,4,1]], N = 4, K = 2
Output: 2
 ```

Note:

```
N will be in the range [1, 100].
K will be in the range [1, N].
The length of times will be in the range [1, 6000].
All edges times[i] = (u, v, w) will have 1 <= u, v <= N and 0 <= w <= 100.
```


### Solution

```
import java.util.PriorityQueue

class Solution {
data class GraphNode(val id: Int, val neighbors: MutableList<GraphNode> = mutableListOf())
fun networkDelayTime(times: Array<IntArray>, N: Int, K: Int): Int {
    val graph = (1..N).map { it to GraphNode(it) }.toMap()
    val weight = mutableMapOf<Pair<Int, Int>, Int>()
    val distance = mutableMapOf<Int, Int>()
    times.forEach { time ->
        graph[time[1]]?.let { graph[time[0]]?.neighbors?.add(it) }
        weight[Pair(time[0], time[1])] = time[2]
    }
    dijkstra(graph, weight, distance, K)
    return distance.map { it.value }.max()?.takeIf { it != Int.MAX_VALUE }?: -1
}

private fun dijkstra(graph: Map<Int, GraphNode>,
                     weight: Map<Pair<Int, Int>, Int>,
                     distance: MutableMap<Int, Int>,
                     sourceId: Int) {
    initializeSingleSource(graph, distance, sourceId)
    val minHeap = PriorityQueue<GraphNode>(Comparator { o1, o2 -> (distance[o1.id]?: 0) - (distance[o2.id]?: 0) })
    minHeap.offer(graph[sourceId]!!)
    while (minHeap.isNotEmpty()) {
        val node = minHeap.poll()
        node.neighbors.forEach {
            val newDist = (distance[node.id]?: 0) + (weight[Pair(node.id, it.id)]?: 0)
            if (distance[it.id]?: 0 > newDist) {
                distance[it.id] = newDist
                graph[it.id]?.let { minHeap.offer(it) }
            }
        }
    }
}

private fun initializeSingleSource(graph: Map<Int, GraphNode>, distance: MutableMap<Int, Int>, sourceId: Int) {
    graph.map { it.key }.forEach { distance[it] = Int.MAX_VALUE }
    distance[sourceId] = 0
}

}

```

### Explanation

으음... 문제좀 풀다가 막혀서 솔루션을 보게되었는뎅.. 다익스트라 알고리즘 오랜만에 들어보았다 내일 다시 풀어보기!