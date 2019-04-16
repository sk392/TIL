### UncommonWordsfromTwoSentences



요번엔 [UncommonWordsfromTwoSentences](https://leetcode.com/problems/uncommon-words-from-two-sentences/)요거를 풀어봐씁니다.

### Problem

We are given two sentences A and B.  (A sentence is a string of space separated words.  Each word consists only of lowercase letters.)

A word is uncommon if it appears exactly once in one of the sentences, and does not appear in the other sentence.

Return a list of all uncommon words. 

You may return the list in any order.

 
```
Example 1:

Input: A = "this apple is sweet", B = "this apple is sour"
Output: ["sweet","sour"]
```

```
Example 2:

Input: A = "apple apple", B = "banana"
Output: ["banana"]
```
 

```
Note:

0 <= A.length <= 200
0 <= B.length <= 200
A and B both contain only spaces and lowercase letters.

```
### Solution

```
class Solution {
    fun uncommonFromSentences(A: String, B: String): Array<String> {
        val hashSetA = HashMap<String,Boolean>()
        val list = A.split(" ").toMutableList().apply{
            addAll(B.split(" "))
        }
        list.forEach{
            hashSetA[it] = hashSetA[it]== null
        }
        val result = arrayListOf<String>()
        hashSetA.forEach{key, value ->
            if(value) result.add(key)
        }

        return result.toTypedArray()
    }
}
```

### Explanation

간단하게 해시셋으로 넣으면서 중복체크 정도만해가지고 해결했다..!!