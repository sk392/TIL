### N-Queens

Hard - [N-Queens](https://leetcode.com/problems/n-queens/)

### Problem

The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space, respectively.


Example 1:

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above



### Solution

```kotlin
class Solution {

    val result = mutableListOf<List<String>>()
    fun solveNQueens(n: Int): List<List<String>> {
        for (y in 0 until n) {
            recursiveSolve(mutableListOf(y), n)
        }
        return result
    }

    fun recursiveSolve(yList: MutableList<Int>, n: Int) {
        val height = yList.size
        if (height >= n) {
            result.add(convertResult(yList, n))
            return
        }
        val findAvailableList = MutableList(n) { i -> i }
        for (i in 0 until yList.size) {
            val currentY = yList[i]
            val diff = height - i

            findAvailableList.remove(currentY)
            findAvailableList.remove(currentY - diff)
            findAvailableList.remove(currentY + diff)
        }
        if (findAvailableList.isEmpty()) return
        for (i in findAvailableList) {
            val newList = mutableListOf<Int>().apply {
                addAll(yList)
                add(i)
            }
            recursiveSolve(newList, n)
        }
    }

    fun convertResult(yList: List<Int>, n: Int): List<String> {
        val result = mutableListOf<String>()

        for (i in yList) {
            val stringBuilder = StringBuilder()
            for (j in 0 until n) {
                val char = if (j == i) {
                    'Q'
                } else {
                    '.'
                }
                stringBuilder.append(char)
            }
            result.add(stringBuilder.toString())
        }
        return result
    }
}
```

### Explanation

유명한 문제를 힌트 없이 한 번에 풀어서 기분 좋음! divide and conquer형태로 풀었다. 처음에는 재귀 형태로 각 노드의 하나씩 둔 후에, 불가능한 위치를 체크해서 가능한 위치에 두어서 해결하려했다. 메모리와 계산방식이 너무 복잡하여 다른 방법을 찾았다.
 Queen은 가로세로, 대각선을 움직일 수 있으니 가로를 기준으로 라인 바이 라인으로 문제를 줄여서 풀면 도움이 된다. 가로 움직임은 각 행별 1개의 퀸만 두는 것으로, 세로 움직임은 이전 퀸들의 포지션을 기억해서 같은 x값을 배제하는 것으로, 대각선은 이전 퀸들의 x값에서 +-행의 차이 값을 배제하는 것으로 로직을 결정하면된다. 가장 헷갈리는게 대각선인데 예를 들어 2번째 행은 1번째 행의 퀸의 위치 에서 +-1을 하면된다. 대각선의 움직임이라는 것이 가로 1칸 + 세로 1칸이 반드시 같이 움직여야하므로 이를 기억해두면 문제를 이해하기 좋다.
