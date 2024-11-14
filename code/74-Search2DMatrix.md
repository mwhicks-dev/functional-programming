# Search a 2D Matrix

From [LeetCode](https://leetcode.com/problems/search-a-2d-matrix):

> You are given an `m x n` integer matrix `matrix` with the following two properties:
> 
> * Each row is sorted in non-decreasing order.
> * The first integer of each row is greater than the last integer of the previous row.
> 
> Given an integer `target`, return *`true` if `target` is in `matrix` or `false` otherwise*.
> 
> You must write a solution in `O(log(m * n))` time complexity.

## Intuition

Any *n*-dimensional array with constant vector sizes can be flattened to a 1D array, so we can access this grid as if it is just one array of size `mn` as long as we can go from our index *0 <= x < mn* to some coordinate pair *(i, j)*. This can be done as *x = ni + j*, so *j = x % n* and *i = (x - j) / n*.

With this in mind, we need simply apply binary search since the 1D array maintains that it is sorted.

## Approach

From our input matrix `matrix`, we can determine `m` and `n` quickly. After this, we will use a function to convert a 1D index *0 <= x < mn* to a coordinate pair *(i, j)*. After this we simply need to perform binary search between the values *0* and *mn - 1*.

## Complexity

* Time complexity: O(log(mn))
* Space complexity: O(1)

## Code

```python
def solution(matrix: list[list[int]], target: int) -> bool:
    rows: int = len(matrix)
    columns: int = len(matrix[0])

    def convert(x: int) -> tuple[int, int]:
        nonlocal columns
        j: int = x % columns
        i: int = (x - j) // columns
        return (i, j)
    
    low: int = 0
    high: int = rows * columns - 1

    while low <= high:
        mid: int = (low + high) // 2
        (row, column) = convert(mid)
        
        value = matrix[row][column]

        if value == target:
            return True
        
        if target > value:
            low = mid + 1
        else:
            high = mid - 1
        
    return False
```