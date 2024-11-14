# Sliding Windows

Sliding window problems are a variation of Two Pointers problems where the pointers can move in the following ways:
* One of the pointers moves right or left.
* Both of the pointers move right or left, but in the same direction.

This creates our "sliding window" - a range between two pointers that can move its origin or change its size, but never both.

To simplify computation, these are often represented with one pointer (referred to here as the origin) and the window size which often defaults to 0 or the size of the scan space.

Sliding windows are commonly used to find extreme subarrays or substrings of an input array or strings that satisfy some condition, and can reduce such problems from O(n^2) to O(n) or O(n log n) in comparison to the use of a nested loop.

## Key Concepts

### Fixed-size Sliding Window

A fixed-size window is simply a window that can slide, but not change size. For instance, given an array `nums` with size `n`, we can use a fixed-size sliding window to find the maximum-valued subarray of size `k < n` in O(n) time:

```python
def fixed_example(nums: list[int], k: int) -> int:
    (start, size, subarray_sum) = (0, 0, 0)
    while size < k:
        subarray_sum += nums[size]
        size += 1
    
    res = subarray_sum
    while start + size < len(nums):
        subarray_sum += nums[start + size]
        subarray_sum -= nums[start]
        start += 1

        res = max(res, subarray_sum)
    
    return res
```

### Variable-size Sliding Window

Now, suppose similarly that we want to find the size of the smallest subarray with a sum greater than `target`, or `-1` if no such subarray exists. In this case, we will start small and change the size as we go, finishing in O(n log n) time:

```python
def variable_example(nums: list[int], target: int) -> int:
    n: int = len(nums)
    initial_sum: dict[int, int] = {0: 0}

    for k in range(1, n):
        (start, size, subarray_sum) = (0, k - 1, initial_sum[k-1])
        subarray_sum += nums[size]
        size += 1
        initial_sum[size] = subarray_sum
        
        while start + size < n:
            if subarray_sum > target:
                return size
            
            subarray_sum += nums[start + size]
            subarray_sum -= nums[start]
            start += 1
    
    return -1
```

Due to the increasing window size, each iteration of `k` will take less time than the last.

## Key Algorithms

None!

## Problems

* [Longest Substring Without Repeating Characters](../code/3-LongestSubstringWithoutRepeatingCharacters.md)
* [Longest Repeating Character Replacement](../code/424-LongestRepeatingCharacterReplacement.md)
* [Sliding Window Maximum](../code/239-SlidingWindowMaximum.md)
