### Word Search


Today's Problem is Medium-  [Word Search](https://leetcode.com/problems/word-search/)

### Problem

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

Example:

```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

### Solution

```
class Solution {
    fun exist(board: Array<CharArray>, word: String): Boolean {
        val visitMap = MutableList(board.size) { MutableList(board[0].size) { false } }
        var result = false

        for (i in 0 until board.size) {
            for (j in 0 until board[0].size) {
                if (board[i][j] == word[0]) {
                    result = result || findNextWord(0, word, board, Pair(i, j), getDeepCopyList(visitMap))
                }
            }
        }

        return result
    }

    fun findNextWord(
        wordIndex: Int,
        word: String,
        board: Array<CharArray>,
        currentPosition: Pair<Int, Int>,
        visitMap: List<MutableList<Boolean>>
    ): Boolean {
        val x = currentPosition.first
        val y = currentPosition.second

        if (x < 0 || x >= board.size || y < 0 || y >= board[0].size || wordIndex >= word.length || board[x][y] != word[wordIndex] || visitMap[x][y]) return false

        visitMap[x][y] = true
        if(wordIndex == word.length-1) return true

        return findNextWord(wordIndex + 1, word, board, Pair(x + 1, y), getDeepCopyList(visitMap))
                || findNextWord(wordIndex + 1, word, board, Pair(x, y + 1), getDeepCopyList(visitMap))
                || findNextWord(wordIndex + 1, word, board, Pair(x - 1, y), getDeepCopyList(visitMap))
                || findNextWord(wordIndex + 1, word, board, Pair(x, y - 1), getDeepCopyList(visitMap))
    }

    fun getDeepCopyList(list: List<MutableList<Boolean>>): List<MutableList<Boolean>> {
        val newList = mutableListOf<MutableList<Boolean>>()
        list.forEach {
            newList.add(it.toMutableList())
        }
        return newList
    }
}
```

### Explanation

처음에 접근할 떈 갈 수 있는 방향중에 허용되는 여러 개의 문자를 어떻게 처리할 것인지를 잘 처리하는게 키 포인트인 것같았는데, 보니까 시작 문자가 정해져있는 것도 아니고 최단거리 길찾기같은거랑은 다른 문제라는걸 깨달았다.

함수를 하나 만들고 지금 현재좌표랑 비교해야하는 다음 문자를 알려주는 게 중요하다고 느꼈고, currentPosition과 wordIndex를 함수에 넣어주고 필요한 값들을 넣어줬다. 또, 여기 조건에서 같은 문자에 대한 셀은 하나만 쓰도록 제한하였기 때문에 VisitMap을 만들어서 해당 좌표에 접근했는지 안했는지에 대해 판단했다. 

P.S - 현재 나의 좌표에서 다음 좌표에 대한 값을 비교하는 것보다 일단 값을 보내주고 현재의 값이 유효한지 판단하여 아닌경우 return false를 쳐주는게 더 코드가 깔끔해질 수 있다.

다른 버전의 findNextWord

```
fun findNextWord(
        wordIndex: Int,
        word: String,
        board: Array<CharArray>,
        currentPosition: Pair<Int, Int>,
        visitMap: List<MutableList<Boolean>>
    ): Boolean {
        val nextWord = if (wordIndex + 1 >= word.length) {
            return true
        } else {
            word[wordIndex + 1]
        }

        val x = currentPosition.first
        val y = currentPosition.second
        var result = false

        visitMap[x][y] = true

        if (x + 1 < board.size && !visitMap[x + 1][y] && board[x+1][y] == nextWord) {
            result = result || findNextWord(wordIndex + 1, word, board, Pair(x + 1, y), getDeepCopyList(visitMap))
        }
        if (y + 1 < board[0].size && !visitMap[x][y + 1] && board[x][y+1] == nextWord) {
            result = result || findNextWord(wordIndex + 1, word, board, Pair(x, y+1), getDeepCopyList(visitMap))
        }
        if (x - 1 >= 0 && !visitMap[x - 1][y]&& board[x-1][y] == nextWord) {
            result = result || findNextWord(wordIndex + 1, word, board, Pair(x - 1, y), getDeepCopyList(visitMap))
        }
        if (y - 1 >= 0 && !visitMap[x][y - 1]&& board[x][y-1] == nextWord) {
            result = result || findNextWord(wordIndex + 1, word, board, Pair(x , y-1), getDeepCopyList(visitMap))
        }
        return result
    }
```