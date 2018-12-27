### ThreeSum

요번엔 [ThreeSum](https://leetcode.com/problems/3sum/)요거를 풀어봐씁니다.

### Problem
Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:

The solution set must not contain duplicate triplets.

Example:

```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

### Solution

```
class Solution {
    fun threeSum(nums: IntArray): List<List<Int>> {
        nums.sort()

        val result: MutableList<List<Int>> = mutableListOf()

		  //i를 기준으로 i+1 부터 nums.size-1까지의 리스트에서 가장 왼쪽, 가장 오른쪽으로 부터 가운데로 좁혀지면서 문제를 해결한다 (정렬되어 있다는 기준 하에)
        for (i in 0 until nums.size) {
			
			  //nums[i]와 nums[i+1]이 아닌 nums[i-1]을 비교하는 바는 전체가 왼쪽부터 오른쪽으로 훑기 시작하므로 -1, -1, -1, 2, 3과 같은 배열이 있을 때 가장 많은 선택지를 가질 수 있는 것은 제일 왼쪽의 숫자이기 때문이다. 
            if(i>0 && nums[i] == nums[i-1]){
                continue
            }
            var left = i+1
            var right = nums.size-1

            while(left<right){
                val sum = nums[i] + nums[left] + nums[right]
                if(sum>0){
                    right--
                }else if( sum < 0){
                    left++
                }else{
                    result.add(result.size, listOf(nums[i],nums[left],nums[right]))
                    //같은 값이라면 가장 오른쪽에 있는 값을 선택한다(왼쪽에서 선택하면 다시 진행방향에 따라 같은 값이 다시 선택 될 수 있으므로
                    while(left<right && nums[left]== nums[left+1]) {
                        left++
                    }
                    
                    //위와 마찬가지
                    while(left<right && nums[right]== nums[right-1]){
                        right--
                    }
                    left++
                    right--
                }
            }

        }

        return result
    }
}
```

### Explanation

으아.. 이거 문제 푸는데 너무 오래 걸렸다.. TwoSum을 푸는 것과 같은 컨셉으로 풀려고 했더니, TwoSum은 정답이 딱 1쌍이었고 그를 통해 A + B = C -> B = C - A를 이용해 해쉬맵으로 풀었다. 하지만 요칭구는 그게 안되고.. 해결 방법을 찾다 결국 솔루션에서 힌트를 얻어서 문제를 풀었다... 쫀심..ㅠㅠ