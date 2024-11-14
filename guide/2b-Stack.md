# Stack

The *stack* is a data structure, typically implemented atop of an array, in which the last item pushed to the stack is the only one accessible unless it is removed (or more simply a last-in first-out, or LIFO, ordering). 

Stacks are used frequently in problems for balancing symbols, optimizing complex array operations, and to put off certain computations until later for use in dynamic programming.

## Key Concepts

Because of the strictly enforced ordering of a stack, it is often the preferred data structure of choice for symbol balancing. 

Consider, for example, any bracket-enclosed language like Java or C. Underneath, their compilers utilize stacks in order to manage scope and ensure that every block of code is closed; for every open bracket (which is pushed to the stack), there must be a close bracket (popped from the stack). Moreover, one cannot close a scope without having opened it (empty stack).

## Key Algorithms

No algorithms, but here is an implementation of a stack data structure in Python.

```python
class Stack:

    array: list

    def __init__(self):
        self.array = []
    
    def push(self, item: Any) -> None:
        self.array.append(item)
    
    def peek(self) -> Any:
        return self.array[-1]
    
    def pop(self) -> None:
        del self.array[-1]
```

Straightforward, but powerful in the right programmer's hands. Depending on implementation, a Stack's pop method may also return the item being popped.

## Key Problems

* [Valid Parentheses](../code/20-ValidParentheses.md)
* [Min Stack](../code/155-MinStack.md)
* [Largest Rectangle in Histogram](../code/84-LargestRectangleInHistogram.md)