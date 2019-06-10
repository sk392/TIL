###  Flood Fill


요번엔 [Flood Fill](https://leetcode.com/problems/flood-fill/)요거를 풀어봐씁니다.

### Problem
An image is represented by a 2-D array of integers, each integer representing the pixel value of the image (from 0 to 65535).

Given a coordinate (sr, sc) representing the starting pixel (row and column) of the flood fill, and a pixel value newColor, "flood fill" the image.

To perform a "flood fill", consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color as the starting pixel), and so on. Replace the color of all of the aforementioned pixels with the newColor.

At the end, return the modified image.

Example 1:

```
Input: 
image = [[1,1,1],[1,1,0],[1,0,1]]
sr = 1, sc = 1, newColor = 2
Output: [[2,2,2],[2,2,0],[2,0,1]]
Explanation: 
From the center of the image (with position (sr, sc) = (1, 1)), all pixels connected 
by a path of the same color as the starting pixel are colored with the new color.
Note the bottom corner is not colored 2, because it is not 4-directionally connected
to the starting pixel.
```

Note:

```
The length of image and image[0] will be in the range [1, 50].
The given starting pixel will satisfy 0 <= sr < image.length and 0 <= sc < image[0].length.
The value of each color in image[i][j] and newColor will be an integer in [0, 65535].
```

### Solution

```
import java.util.*


class Solution {
    fun floodFill(image: Array<IntArray>, sr: Int, sc: Int, newColor: Int): Array<IntArray> {
        val color = image[sr][sc]
        val isNotDrawArray = Array(image.size){BooleanArray(image[0].size){true}}
        val queue = ArrayDeque<Pair<Int,Int>>()
        queue.push(Pair(sr,sc))
        while (queue.isNotEmpty()){
            val temp = queue.poll()
            if(image.size > temp.first+1 && isNotDrawArray[temp.first+1][temp.second] &&image[temp.first+1][temp.second] == color){
                queue.push(Pair(temp.first+1,temp.second))
                isNotDrawArray[temp.first+1][temp.second] = false
            }
            if(image[0].size > temp.second+1 && isNotDrawArray[temp.first][temp.second+1] && image[temp.first][temp.second+1] == color){
                queue.push(Pair(temp.first,temp.second+1))
                isNotDrawArray[temp.first][temp.second+1] = false
            }
            if(0 <= temp.first-1 && isNotDrawArray[temp.first-1][temp.second] && image[temp.first-1][temp.second] == color){
                queue.push(Pair(temp.first-1,temp.second))
                isNotDrawArray[temp.first-1][temp.second] = false
            }
            if(0 <= temp.second-1 && isNotDrawArray[temp.first][temp.second-1]  && image[temp.first][temp.second-1] == color){
                queue.push(Pair(temp.first,temp.second-1))
                isNotDrawArray[temp.first][temp.second-1] = false
            }
            image[temp.first][temp.second] = newColor
        }

        return image
    }
}
```

### Explanation

간단한 문젠데, 색칠 하는 색상에 따라서 무한 루프에 돌수도 있으므로 한번 체크한 좌표에는 체크 되지 않도록 array를 하나 더 둬서 확인한다.