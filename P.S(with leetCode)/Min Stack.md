### Min Stack



Today's Problem is Easy - [Min Stack](https://leetcode.com/problems/min-stack/)

### Problem

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

push(x) -- Push element x onto stack.
pop() -- Removes the element on top of the stack.
top() -- Get the top element.
getMin() -- Retrieve the minimum element in the stack.
 

Example 1:

```
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
``` 

Constraints:

```
Methods pop, top and getMin operations will always be called on non-empty stacks.
```

### Solution

```
class MinStack() {
    private val stack = mutableListOf<Int>()
    private val index: Int
        get() = stack.size - 1
    private val minMap = mutableMapOf<Int, Int>()

    fun push(x: Int) {
        stack.add(x)
        minMap[index] = Math.min(minMap[index - 1] ?: Int.MAX_VALUE, x)
    }

    fun pop() {
        stack.removeAt(index)
    }

    fun top(): Int {
        return stack.last()
    }

    fun getMin(): Int {
        return minMap[index] ?: 0
    }
}

```

### Explanation

so~ Easy~~~ 여러 가지 방법으로 풀  수 있겠지만 내부테 Map을 하나 둬서 해결해봤다.