### FindTheTownJudge


요번엔 [FindTheTownJudge](https://leetcode.com/problems/find-the-town-judge/)요거를 풀어봐씁니다.

### Problem
In a town, there are N people labelled from 1 to N.  There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:

The town judge trusts nobody.
Everybody (except for the town judge) trusts the town judge.
There is exactly one person that satisfies properties 1 and 2.
You are given trust, an array of pairs trust[i] = [a, b] representing that the person labelled a trusts the person labelled b.

If the town judge exists and can be identified, return the label of the town judge.  Otherwise, return -1.

 

```
Example 1:

Input: N = 2, trust = [[1,2]]
Output: 2
```

```
Example 2:

Input: N = 3, trust = [[1,3],[2,3]]
Output: 3
```

```
Example 3:

Input: N = 3, trust = [[1,3],[2,3],[3,1]]
Output: -1
```

```
Example 4:

Input: N = 3, trust = [[1,2],[2,3]]
Output: -1
```

```
Example 5:

Input: N = 4, trust = [[1,3],[1,4],[2,3],[2,4],[4,3]]
Output: 3
```
 
```
Note:

1 <= N <= 1000
trust.length <= 10000
trust[i] are all different
trust[i][0] != trust[i][1]
1 <= trust[i][0], trust[i][1] <= N
```

### Solution

```
class Solution {
    fun findJudge(N: Int, trust: Array<IntArray>): Int {
    if ( N ==1) return 1
    var result = -1
    val resultMap = mutableMapOf<Int, Int>()
    trust.forEach { array ->
        resultMap[array[0]] = -1
        when(resultMap[array[1]]){
            null -> resultMap[array[1]] = 1
            -1 -> {}
            else -> resultMap[array[1]] = resultMap[array[1]]!! + 1
        }
    }

    resultMap.forEach{key , value ->
        if(value== N-1 && result != -1 ){
            result = -1
            return@forEach
        }
        if(value== N-1){
            result = key
        }
    }

    return result
    }
}
```

### Explanation

음.. 밑에 resultMap의 로직을 수정하면 좀 더 시간을줄일 수 있을 것같다 DP로 해결하면 되려나?