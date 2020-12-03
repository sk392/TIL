### Maximal Square



Today's Problem is Medium - [Maximal Square](https://leetcode.com/problems/maximal-square/)

### Problem


Given an m x n binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

 

Example 1:

![](https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg)
```
Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
Output: 4
```

Example 2:

![](https://assets.leetcode.com/uploads/2020/11/26/max2grid.jpg)

```
Input: matrix = [["0","1"],["1","0"]]
Output: 1
```

Example 3:

```
Input: matrix = [["0"]]
Output: 0
```


### Solution

```java
class Solution {
    private int maxResult = 0;
    public int maximalSquare(char[][] matrix) {
        for(int i = 0; i< matrix.length; i++ ){
            for(int j = 0; j<matrix[0].length; j++){
                if(matrix[i][j] == '1') checkMaxSqure(i,j,matrix);
            }
        }
        return maxResult;
    }

    private void checkMaxSqure(int startX,int startY,char[][] matrix){
        int minimumWidth = matrix.length - startX;
        int minimumHeight = matrix[0].length - startY;
        int minimumSize = minimumWidth > minimumHeight ? minimumHeight : minimumWidth;

        for( int i = 1; i <= minimumSize; i++){
            for(int k = startX; k< startX + i; k++){
                for(int j = startY; j<startY + i;j++){
                    if(matrix[k][j] != '1') return;
                }
            }
            maxResult = Math.max(maxResult,i*i);
        }
    }
}
```

### Explanation

오랜만에 하이퍼 커넥트 코딩테스트 때문에 JAva로 문제를 풀고 있었다..  새로운 이중 어레이를 만드는 방법을 몰라서 한참 당황했다..

무튼 문제는 해당 1인 좌표를 찾아서 해당 좌표에서 스퀘어를 확장할 수 있는지 확인한다.

좌표를 찾을 때 오른쪽, 아래 순서로 서치하므로 스퀘어를 만들 때도 위, 왼쪽으로는 가지 않아도 된다.

비슷한 문제로 섬의 최대면적같은걸 구하는 문제가 있는데, 거기선 visitMap등을 활용해서 역순으로 찾아가지 않도록 하는 것이 중요한데 비애 여기는 스퀘어 그리느라 방문했던 좌표도 해당 좌표에서 더 큰 스퀘어가 만들어질 수 있기 때문.