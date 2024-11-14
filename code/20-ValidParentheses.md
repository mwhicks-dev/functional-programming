# Valid Parentheses

From [LeetCode](https://leetcode.com/problems/valid-parentheses/):

> Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.
> 
> An input string is valid if:
> 
> 1. Open brackets must be closed by the same type of brackets.
> 1. Open brackets must be closed in the correct order.
> 1. Every close bracket has a corresponding open bracket of the same type.

## Intuition

Balancing symbols is a very simple stack application.

## Approach

We will utilize a list as a stack in order to ensure that `s` is appropriate. Namely, for each `)`, `]`, or `}`, it must be the case that there is a `(`, `[`, or `{` respectively on the top of the stack, else `s` is invalid. Furthermore, if the stack is not emptied by the end of `s`, then this is also invalid.

A hash map will also be used to simplify some code.

## Complexity

* Time complexity: O(n)
* Space complexity: O(n)

## Code

```python
def solution(s: str) -> bool:
    mapping: dict[str, str] = { ')': '(', ']': '[', '}': '{' }

    stack: list[str] = []
    for c in s:
        if c in mapping:
            if len(stack) == 0 or stack[-1] != mapping[c]:
                return False
            del stack[-1]
        else:
            stack.append(c)
    return len(stack) == 0
```