###  Distant Barcodes



요번엔 [Distant Barcodes
](https://leetcode.com/problems/distant-barcodes/)요거를 풀어봐씁니다.

### Problem

In a warehouse, there is a row of barcodes, where the i-th barcode is barcodes[i].

Rearrange the barcodes so that no two adjacent barcodes are equal.  You may return any answer, and it is guaranteed an answer exists.

 

Example 1:

```
Input: [1,1,1,2,2,2]
Output: [2,1,2,1,2,1]
```

Example 2:

```
Input: [1,1,1,1,2,2,3,3]
Output: [1,3,1,3,2,1,2,1]
``` 

Note:

```
1 <= barcodes.length <= 10000
1 <= barcodes[i] <= 10000
```

### Solution

```
class Solution {


    fun rearrangeBarcodes(barcodes: IntArray): IntArray {
        val map = mutableMapOf<Int, Int>()

        barcodes.forEach {
            map[it] = (map[it] ?: 0) + 1
        }
        val result = IntArray(barcodes.size)

        var tempBarcode = 0
        for (i in 0 until barcodes.size) {
            var barcode = getMostValueWithOutTheNumber(map, tempBarcode)
            map[barcode] = (map[barcode] ?: 0) - 1
            tempBarcode = barcode

            result[i] = barcode
        }

        return result
    }

    fun getMostValueWithOutTheNumber(map: Map<Int, Int>, withOutNumber: Int): Int {
        var maxValue = 0
        var maxKey = 0

        map.forEach {
            if (maxValue < it.value && it.key != withOutNumber) {
                maxKey = it.key
                maxValue = it.value
            }
        }
        return maxKey
    }
}
```

### Explanation

띠용ㅋㅋㅋ 일단 문제를 푸는 룰을 개수가 많은 숫자부터 차례로 하나씩 값을 채워주면 되는 것같았고.. 더 나은 튜닝법이 있을 것같았지만 일단 맵에 각 숫자의 개수를 넣어주고 많은 친구부터 하나씩 빼서 결과를 냈는데.. 이게 100/ 100퍼센트가 떠서 당황스럽네.. 이게 젤 빠르다는걸까!