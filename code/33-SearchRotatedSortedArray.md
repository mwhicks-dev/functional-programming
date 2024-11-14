# Search in Rotated Sorted Array

From [LeetCode](https://leetcode.com/problems/search-in-rotated-sorted-array/):

> There is an integer array `nums` sorted in ascending order (with **distinct** values).
> 
> Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.
> 
> Given the array `nums` **after** the possible rotation and an integer `target`, return *the index of `target` if it is in `nums`, or `-1` if it is not in `nums`*.
> 
> You must write an algorithm with `O(log n)` runtime complexity.

## Intuition

Referring to the [Binary Search guide](../guide/3a-BinarySearch.md), we already know that we can search a rotated array in O(log n) time as long as we know at what index it was rotated, but since we do not, we will need to find that in O(log n) time or better as well. As such, we should prepare to do this using binary search. Once this value is found, we can search the array freely.

## Approach

Let us first seek our integer value `shift` such that `nums[shift]` is the lowest value in the sorted list. In order to do this, we will take a look at all possible rotations of the array `[1 2 3 4 5]`:

0. [1 2 3 4 5]: `shift` is `0`
0. [5 1 2 3 4]: `shift` is `1`
0. [4 5 1 2 3]: `shift` is `2`
0. [3 4 5 1 2]: `shift` is `3`
0. [2 3 4 5 1]: `shift` is `4`

From this example, it is clear how we should organize our search based on the first step. We can see that if `nums[low]` <= `nums[high]`, then `shift` must be `low` as `nums[low] < nums[(low - 1) % n]`. This is our base case, and our subproblems will maintain this property.

Otherwise, we can make some assumptions about where `shift` is based on the values at `mid` compared to those at `low`, and `high`.
* If `nums[mid]` is greater than or equal to both `nums[low]` and `nums[high]`, then `shift` is somewhere between `mid + 1` and `high`.
* If `nums[mid]` is less than either `nums[low]` and `nums[high]`, then `shift` is somewhere between `low` and `mid`.

These recursive cases account for weird boundary cases where mid is low and/or high, things that someone writing a solution needs to consider.

Once `shift` is determined, we can perform binary search on the rotated sorted array.

## Complexity

* Time complexity: O(log n)
* Space complexity: O(1)

## Code

```python
def solution(nums: list[int], target: int) -> int:
    n: int = len(nums)
    
    # search for shift
    (low, high) = (0, n - 1)

    shift: int = -1
    while low <= high:
        mid: int = (high + low) // 2
        
        if nums[low] <= nums[high]:
            shift = low
            break
        
        if nums[mid] >= nums[low] and nums[mid] >= nums[high]:
            low = mid + 1
        else:
            high = mid

    (low, high) = (0, n - 1)
    while low <= high:
        mid: int = (high + low) // 2
        adjusted: int = (mid + shift) % n
        value: int = nums[adjusted]

        if value == target:
            return adjusted
        
        if target < value:
            high = mid - 1
        else:
            low = mid + 1
    
    return -1
```
