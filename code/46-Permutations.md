# Permutations

From [LeetCode](https://leetcode.com/problems/permutations/):

> A **permutation** is a rearrangement of all the elements of an array.
> 
> Given an array `nums` of distinct integers, return all the possible 
`permutations`. You can return the answer in ***any order***.

## Intuition

We can think about `nums` as a list of potential numbers to add at any given time, and select one add a time to remove from `nums` and add to the back of our permutation. Since we are building our solution one step at a time like this, it would indicate that we are working with a backtracking solution.

## Approach

We will encode our candidate with the partial solution, a list of `nums` in some given order, and the remaining ones. Since there should be no possible way to reach two identical permutations, we don't need to worry about uniqueness.

## Complexity

- Time complexity: O(n!)
- Space complexity: O(n * n!)

## Code

```python
class Candidate:
    def __init__(self, remaining: set[int], partial: list[int]):
        self.remaining = remaining
        self.partial = partial
    
    def extend(self):
        res = []
        for item in self.remaining:
            next_remaining = self.remaining.copy()
            next_remaining.remove(item)

            next_partial = self.partial.copy()
            next_partial.append(item)

            res.append(Candidate(next_remaining, next_partial))
        return res

def solution(nums: list[int]) -> list[list[int]]:
    res: list[list[int]] = []

    def backtracking(partial: Candidate) -> None:
        if len(partial.partial) == len(nums):
            nonlocal res
            res.append(partial.partial)
        else:
            for candidate in partial.extend():
                backtracking(candidate)
    
    backtracking(Candidate(nums, []))

    return res
```
