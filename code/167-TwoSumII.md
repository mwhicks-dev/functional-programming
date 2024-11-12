# Two Sum II - Input Array Is Sorted

From [LeetCode](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/):

> Given a **1-indexed** array of integers `numbers` that is already ***sorted in non-decreasing order***, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.
> 
> Return *the indices of the two numbers, `index1` and `index2`, **added by one** as an integer array `[index1, index2]` of length 2*.
> 
> The tests are generated such that there is **exactly one solution**. You **may not** use the same element twice.
> 
> Your solution must use only constant extra space.

## Intuition

Since the list is sorted, we are able to use a two pointers approach. Admittedly, this can only be determined on-the-spot with some prior experience, but the reason that it works is because of the sorting - with our two pointers, moving one of them left will decrease `numbers[index1]` + `numbers[index2]`, and moving one of them right will increase it.

We will begin with `index1` at the beginning of the list and `index2` at the end. Let `value` = `numbers[index1]` + `numbers[index2]`. If `value` < `target`, shift `index1` one space to the right; if `index2` > `target`, shift `index2` one space to the left. If `value` = `target`, return `[index1 + 1, index2 + 1]` (as the output format is for a 1-indexed list).

At first, this may seem like it could miss some important cases, but we can prove mathematically that it does not. A [proof](https://drive.google.com/file/d/1du9yr2LMG9Gtia_o5TVeu8BXISINJzD5/view?usp=sharing) to a similar problem, [Container With Most Water](https://leetcode.com/problems/container-with-most-water/description/), outlines a similar process by contradiction.

## Complexity

* Time Complexity: O(n)
* Space Complexity: O(1)

## Code

```python
def solution(numbers: list[int], target: int) -> tuple[int, int]:
    left: int = 0
    right: int = len(numbers) - 1

    value = numbers[left] + numbers[right]
    while value != target:
        if value < target:
            left += 1
        else:
            right -= 1
        value = numbers[left] + numbers[right]
    
    return (left + 1, right + 1)
```
