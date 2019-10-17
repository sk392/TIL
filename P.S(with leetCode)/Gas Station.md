### Gas Station



요번엔 [CGas Station](https://leetcode.com/problems/gas-station/)요거를 풀어봐씁니다.

### Problem

There are N gas stations along a circular route, where the amount of gas at station i is gas[i].

You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from station i to its next station (i+1). You begin the journey with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1.

Note:

If there exists a solution, it is guaranteed to be unique.
Both input arrays are non-empty and have the same length.
Each element in the input arrays is a non-negative integer.
Example 1:

```
Input: 
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]

Output: 3

Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
```

Example 2:

```
Input: 
gas  = [2,3,4]
cost = [3,4,3]

Output: -1

Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.
```

### Solution

```
class Solution {

    var canCompleteCircuitStartingPosition: Int = -1
    fun canCompleteCircuit(gas: IntArray, cost: IntArray): Int {
        var startPositive = true
    for (i in 0 until gas.size) {
            if (gas[i] >= cost[i] ) {
                if(startPositive){
                    startPositive = false
                    moveToNextStation(gas, cost, i)
                }
            }else{
                startPositive = true
            }
        }

        return canCompleteCircuitStartingPosition
    }

    fun moveToNextStation(gas: IntArray, cost: IntArray,startPosition: Int) {
        var hasGas = 0
        var nextPosition = startPosition

        do {
            hasGas += gas[nextPosition]
            if (hasGas >= cost[nextPosition]) {
                hasGas -= cost[nextPosition]
                nextPosition = if (nextPosition + 1 >= gas.size) {
                    0
                } else {
                    nextPosition + 1
                }
            }else{
               return
            }
        } while (nextPosition != startPosition)

        canCompleteCircuitStartingPosition = startPosition
    }
}
```

### Explanation

맨처음엔 재귀함수를 통한 DP로 문제를 해결하려고 헀는데, StackOverFlow가 발생해서 생각해보니 재귀가 아니어도 가능할듯해서 이렇게 해당 시작지점에서 포문으로 한바꾸 도는걸로 지정

startpositive플래그가 튜닝용으로 들어갔는데, 오른쪽 한방향으로 순환하고 시작 가장 왼쪽에서 포지티브가 여러개 연결되어 있는 경우는 가장 왼쪽에서 부터 시작하는게 가장 베스트고 가장 왼쪽이 안된다면 그 후에 오른쪽 것들은 의미가 없기 때문에 이렇게 동작.