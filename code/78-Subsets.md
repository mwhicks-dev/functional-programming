# Subsets

From [LeetCode](https://leetcode.com/problems/subsets/description/):

> A subset of an array is a selection of elements (possibly none) of an array.
> 
> Given an integer array `nums` of **unique elements**, return all *possible subsets (the power set)*.
> 
> The solution set must not contain duplicate subsets. Return the solution in any order.

## Backtracking Solution

### Intuition

We want to find all of the unique subsets of 0, 1, ..., n elements. This can be done in an inductive manner; we begin with the only unique subset 0 one elements, `{}`. From `{}`, we can generate all of the unique subsets of 1 element by, for each member `x` in the set, adding `x` to `{}`, then do the same by adding each `y` with `y != x` to `{x}` to generate subsets of size 2, and so on. Because we are building our solution in this way, we should like to use backtracking.

### Approach

We already know to use backtracking as we are building our solution one step at a time, but we need to know how to hash our solutions so that they are not done twice, as well as how to ensure that we do not add the same element to a subset twice.

We can keep our solutions in sorted order and hash them to a string in order to prevent us from visiting the same partial candidate twice.

The easiest way to do the latter is probably to select elements using a monotonically increasing index that is passed as a part of each candidate solution. This way, will require that the pointer is increased during each iteration. Since the subsets are unique, this would also stop us from needing to keep our solution strings in sorted order.

### Complexity

- Time complexity: O(2^n)
- Space complexity: O(2^n)

### Code

```python
class Candidate:
    def __init__(self, partial: list[int] = [], pointer: int = -1):
        self.partial: list[int] = partial
        self.pointer: int = pointer
    
    def extend(self, nums: list[int]) -> list:
        res: list[Candidate] = []
        for index in range(self.pointer + 1, len(nums)):
            res.append(
                Candidate(
                    self.partial.copy() + [nums[index]],
                    index
                )
            )
        return res
    
    def __hash__(self) -> int:
        string_representation: str = ','.join([str(x) for x in self.partial])
        return hash(self.partial)

def solution(nums: list[int]) -> list[list[int]]:
    res: list[list[int]] = []

    visited: set[int] = set()
    def backtracking(candidate: Candidate) -> None:
        # check if visited already
        nonlocal visited
        hv: int = hash(candidate)
        if hv in visited:
            return
        visited.add(hv)

        # always solution - add partial to res
        nonlocal res
        res.append(candidate.partial)

        # go through extensions
        nonlocal nums
        for extension in candidate.extend(nums):
            backtracking(extension)
    
    backtracking(Candidate())

    return res
```

This solution went through a few iterations before I settled on a lazy hash function, but sometimes overengineering can be bad.

Speaking of overengineering, you definitely do not need to write a class for your candidate space!
