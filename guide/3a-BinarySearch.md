# Binary Search

*Binary Search* is an algorithm used to search for a value within a sorted array by, at each iteration, dividing the search interval in half. This starts with the entire interval; based on the middle value of the interval, the search will next be divided into the half that may possibly contain the target. (That is, if the target is less than the middle we search the left half next, and if the target is greater than the middle we search the right half next.)

Binary search can be done iteratively or recursively and should return the index of the target value within the list, 

## Key Concepts

Binary Search is similar to a divide and conquer algorithm such as [merge sort](./1-ArraysHashing.md), though it is a specialization where one of the two subproblems within step has O(1) runtime. For this reason, Binary Search runs in O(log n) time so long as the list it is being performed on has random access (any index can be accessed in O(1) time).

## Key Algorithms

As aforementioned, binary search can be done both iteratively and recursively:

```python
def iterative(array: list[int], target: int) -> int:
    low: int = 0
    high: int = len(array) - 1

    while low <= high:
        mid: int = (low + high) // 2
        value: int = array[mid]

        if value == target:
            return mid
        elif value > target:
            high = mid - 1
        else:
            low = mid + 1
    
    return -1

def recursive(array: list[int], target: int, low: int = 0, high: int = len(array) - 1) -> int:
    # base case
    if high < low:
        return -1
    mid: int = (low + high) // 2
    value: int = array[mid]

    if value == target:
        return mid
    
    # recursive case
    if value > target:
        return recursive(array, target, low, mid - 1)
    else:
        return recursive(array, target, mid + 1, high)
```

A common functional programming scenario involves a sorted list of length *n* such that the smallest value is located at index *0 < k < n* and the largest at index *(k - 1) % n*. Binary search, of course, can still be applied in O(log n) time.

```python
k: int

def iterative(array: list[int], target: int) -> int:
    n: int = len(array)

    low: int = 0
    high: int = n - 1

    while low <= high:
        mid: int = (low + high) // 2
        res: int = (mid + k) % n
        value: int = array[res]

        if value == target:
            return res
        elif value > target:
            high = mid - 1
        else:
            low = mid + 1
    
    return -1

def recursive(array: list[int], target: int, low: int = 0, high: int = len(array) - 1) -> int:
    # base case
    if high < low:
        return -1
    mid: int = (low + high) // 2
    res: int = (mid + k) % n
    value: int = array[res]

    if value == target:
        return res
    
    # recursive case
    if value > target:
        return recursive(array, target, low, mid - 1)
    else:
        return recursive(array, target, mid + 1, high)
```

## Problems

* [Search a 2D Matrix](../code/74-Search2DMatrix.md)
* [Search in Rotated Sorted Array](../code/33-SearchRotatedSortedArray.md)

