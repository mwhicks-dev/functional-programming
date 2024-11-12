# Two Sum

From [LeetCode](https://leetcode.com/problems/two-sum/description/):

> Given an array of integers `nums` and an integer `target`, return *indices of two numbers such that they add up to `target`*.
> 
> You may assume that each input would have ***exactly* one solution**, and you may not use the *same* element twice.
> 
> You can return the answer in any order.

## Intuition

We can utilize hashing in order to solve this problem in one pass at the expense of space. Create a hash map `M`. We will iterate over each index `i` in `nums`, where `value` is the value of `nums` at `i`. Set `M[value] = i`.

At each iteration, let `x` be a value such that `value` + `x` = `target`. Then if `x` has been visited previously, its index will be stored at `M[x]`. In this case, return `(M[x], i)`; otherwise, continue.

Since there is exactly one solution, this branch should be reached eventually.

## Complexity

- Time complexity: O(n)
- Space complexity: O(n)

## Code

```python
def solution(nums: list[int], target: int) -> tuple[int, int]:
    M: dict[int, int] = {}

    for idx in range(0, len(nums)):
        # determine value, x
        value: int = nums[idx]
        x: int = target - value

        # check for solution
        if x in M:
            return (M[x], idx)
        
        # update hashmap to enable possible future solution
        M[value] = idx
    
    return None
```
