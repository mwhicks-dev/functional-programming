# Min Stack

From [LeetCode](https://leetcode.com/problems/min-stack):

> Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
> 
> Implement the `MinStack` class:
> 
> `MinStack()` initializes the stack object.
> `void push(int val)` pushes the element `val` onto the stack.
> `void pop()` removes the element on the top of the stack.
> `int top()` gets the top element of the stack.
> `int getMin()` retrieves the minimum element in the stack.
> You must implement a solution with `O(1)` time complexity for each function.

## Intuition

There will, of course, be a stack underneath responsible for generic stack behavior. In order to keep track of the minimum, we will have another equally sized stack which tracks what the smallest element is at each push.

## Approach

Our `stack` will behave as a normal stack; the `min_stack` will have the following behaviors on push and pop:
* During a push, the minimum of `min_stack[-1]` and `val` will be pushed to the `min_stack`.
* When the `stack` is popped, so will be the `min_stack`.

This will allow each operation to run in O(1) time at the expense of some space, though space will still be O(n).

## Complexity

* Time complexity: O(1) for each operation
* Space complexity: O(n)

## Code

```python
class MinStack:

    stack: list[int]
    min_stack: list[int]

    def __init__(self):
        self.stack = []
        self.min_stack = []
    
    def push(self, val: int) -> None:
        self.stack.append(val)
        if len(self.min_stack) == 0:
            self.min_stack.append(val)
        else:
            self.min_stack.append(min(self.min_stack[-1], val))
    
    def pop(self) -> None:
        del self.stack[-1]
        del self.min_stack[-1]
    
    def top(self) -> int:
        return self.stack[-1]
    
    def getMin(self) -> int:
        return self.min_stack[-1]
```