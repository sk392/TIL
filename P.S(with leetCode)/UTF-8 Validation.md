### UTF-8 Validation



오늘의 문제는!  [UTF-8 Validation](https://leetcode.com/problems/utf-8-validation/)

### Problem
A character in UTF8 can be from 1 to 4 bytes long, subjected to the following rules:

For 1-byte character, the first bit is a 0, followed by its unicode code.
For n-bytes character, the first n-bits are all one's, the n+1 bit is 0, followed by n-1 bytes with most significant 2 bits being 10.
This is how the UTF-8 encoding would work:

```
   Char. number range  |        UTF-8 octet sequence
      (hexadecimal)    |              (binary)
   --------------------+---------------------------------------------
   0000 0000-0000 007F | 0xxxxxxx
   0000 0080-0000 07FF | 110xxxxx 10xxxxxx
   0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
   0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
```

Given an array of integers representing the data, return whether it is a valid utf-8 encoding.

Note:
The input is an array of integers. Only the least significant 8 bits of each integer is used to store the data. This means each integer represents only 1 byte of data.

Example 1:

```
data = [197, 130, 1], which represents the octet sequence: 11000101 10000010 00000001.

Return true.
It is a valid utf-8 encoding for a 2-bytes character followed by a 1-byte character.
```

Example 2:

```
data = [235, 140, 4], which represented the octet sequence: 11101011 10001100 00000100.

Return false.
The first 3 bits are all one's and the 4th bit is 0 means it is a 3-bytes character.
The next byte is a continuation byte which starts with 10 and that's correct.
But the second continuation byte does not start with 10, so it is invalid.
```

### Solution

```
class Solution {
    fun validUtf8(data: IntArray): Boolean {
        val bits = mutableListOf<String>().apply {
            data.forEach {
                val bit = if (it < 128) {
                    var str = Integer.toBinaryString(it)
                    for (i in 0 until (8 - str.length)) {
                        str = "0$str"
                    }

                    str
                } else {
                    Integer.toBinaryString(it)
                }
                this.add(bit)
            }
        }
        var bitCount = 0

        bits.forEach { bit ->
            if (bitCount == 0) {
                val zeroIndex = bit.toCharArray().indexOfFirst { it == '0' }

                bitCount = when {
                    zeroIndex < 0 || zeroIndex > 4 || zeroIndex == 1 -> -1
                    zeroIndex > 1 -> zeroIndex - 1
                    else -> zeroIndex
                }

            } else if (bitCount > 0) {
                bitCount = if (bit.subSequence(0, 2) == "10") {
                    bitCount - 1
                } else {
                    -1
                }
            }
        }

        return bitCount == 0
    }
}
```

### Explanation

이 문제는 첫번째 바이트에 따라 뒤에 바이트 구성이 정해지기 때문에 뒤에 바이트 구성이 정해지는 개수를 카운팅하는 bitCount로 계산한다. 또, 128이하는 숫자 규격에 맞지 않으니 앞에 0을 갯수에 맞게 붙여준다.