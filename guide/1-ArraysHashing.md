# 1 Arrays and Hashing

The fundamental problem space, Arrays and Hashing involves the storage and retrieval of data using collections, such as arrays, lists, sets, and maps/dictionaries.

Arrays are contiguous blocks of memory that hold a fixed number of data points with a specific size. For instance, an array with 4 16-bit integers will take approximately 64 bits of uninterrupted memory. Increasing the capacity of an array requires the allocation of a new, larger block of uninterrupted memory. 

Lists, which handle this memory management automatically, often grab larger chunks of memory than needed to avoid doing this too often. 

Sets and (hash) maps/dictionaries provide more complex behavior, often at the expense of memory but in exchange for speed and specialization.

## Key Concepts

### Maps/Dictionaries

**Definition**. Let *X* and *Y* be some collection. A *map*, or *dictionary*, is a data structure *M* such that given *x* in *X* and *y* in *Y*, a programmer can assign *M[x] = y*. This allows some desired value in *Y* to be assigned to a particular value in *X*.

A value *u* in *U* is *hashable* if it can be converted to an integer. Ideally, it is best to have unique hashing; that is, if *u* and *v* are in *U*, and *H* hashes members in *U* to an integer, *H(u) = H(v)* if and only if *u = v*. Rather, if *u* and *v* are not equal, then *H(u)* and *H(v)* should be different integers.

*Hash maps* are a type of map implemented using hashing. Let *M* be a hash map containing an array *A* of *Y* valuues. If *x* is hashable, then by hashing *x* to a unique index *i* in *A*, we can access *M[x]* as *A[H(x)] = A[i]*. This allows for map insert, read, and delete operations to be performed in O(1) time at the expense of keeping the large hash array *A*.

To reduce space consumption, developers may keep a smaller array and utilize modulus for hashing to its indices. Sometimes, it is not possible for hashes to be unique. In the event that a hash map tries to insert two values in *X* to the same hashed index, this is called a *collision*. These are slightly complicated topics that you often will not need to know for functional programming interviews, but that you should read more on your own to become a better programmer.

Python's dictionaries are hash maps.

### Sets

**Definition**. Let *X* be some collection. A *set* in programming is a data structure that contains non-duplicate members of *X*, often written in mathematical set notation (e.g. {1, 2, 3, 4}). For example, {1, 1, 3} is not a valid set as it contains the value *1* twice. The key operations for a set are to *insert* and *remove* values as well as to check if some value *x* in *X* is contained in the set *S*.

Hashing can also be used in a straightforward way to implement a set. For simplicity, think about how you could implement a "hash set" *S* over *X* using a hash map *M* from *X* to a boolean:
* To insert *x* into *S*, set *M[x] = 1*.
* To remove *x* from *S*, set *M[x] = 1* or remove *x* from *M*.
* To check if *x* is in *S*, check if *x* is in *M*, and if so, if *M[x]* is *1*; if so, it is; otherwise no.

This has similar implications for time and space complexity.

## Key Algorithms

### Frequency Counting

Given an array *A* over *X*, it is regularly useful to measure the frequencies of the values of *X* contained in *A*. This can be done using hashing so long as *X* is a hashable space (which it usually is).

```python
def frequency(array: list[X]) -> dict[X, int]:
    res: dict[X, int] = {}

    for item in array:
        if item not in res:
            res[item] = 0
        res[item] += 1
    
    return res
```

This function uses hashing to determine the frequency of array elements in O(n) time. Any items not in the output dictionary have a frequency of *0*.

### Sorting

**Definition**. Let *A* be an array of elements over a space *X*, where members of *X* can be compared using relational operators, or at the bare minimum *<*. Furthermore, let *|A|* be the number of elements in *A*. A sorting algorithm *S* arranges the elements of *A* such that, for each index *0 < i < |A|*, *A[i]* is not less than *A[i-1]*. That is, *A* increases (non-strictly) from left to right.

There are many sorting algorithms that you should be aware of, even if you do not remember their algorithms. The easiest, and slowest, among basic algorithms is *bubble sort*:

```python
def bubble_sort(array: list[X]) -> None:
    done: bool = False
    while not done:
        done = True
        for idx in range(1, len(array)):
            if array[idx] < array[idx - 1]:
                tmp = array[idx]
                array[idx] = array[idx - 1]
                array[idx - 1] = tmp
                done = False
```

This sorting algorithm takes elements and propagates them towards the back of the list until the list is sorted, but in the worst case, the runs in *O(n^2)* time if *|A| = n*. For instance, let *A = [4 3 2 1]*. In this case, we have the following sequence of steps:

0. [4 3 2 1]  (compare 4 and 3)
0. [3 4 2 1]  (compare 4 and 2)
0. [3 2 4 1]  (compare 4 and 1)
0. [3 2 1 4]  (compare 3 and 2)
0. [2 3 1 4]  (compare 3 and 1)
0. [2 1 3 4]  (compare 3 and 4)
0. [2 1 3 4]  (compare 1 and 2)
0. [1 2 3 4]  (compare 1 and 2)
0. [1 2 3 4]  (compare 2 and 3)
0. [1 2 3 4]  (compare 3 and 4)

This case, where the list is already sorted in the opposite order, is the worst case scenario, and will scale proportionally to the squared length of the input list.

Merge sort is a sorting technique in which the array is partitioned into small subproblems and sorted at the lowest level, propagating back upwards. For instance:

0. [4 3 2 1] is split in half into [4 3] and [2 1]
0. [4 3] is divided into [4] and [3]
0. [4] and [3] are merged as [3] and [4]
0. [2 1] is divided into [2] and [1]
0. [2] and [1] are merged into [1 2]
0. [3 4] and [1 2] are merged into [1 2 3 4]

```python
def merge_sort(array: list[X]) -> None:
    def merge(left: int, right: int) -> None:
        nonlocal array

        mid = (left + right) // 2
        (left_array, right_array) = ([], [])

        tmp = left_array
        for idx in range(left, right + 1):
            if idx == mid + 1:
                tmp = right_array
            tmp.append(array[idx])
        
        (i, j, k) = (0, 0, left)
        while i < len(left_array) and j < len(right_array):
            if left_array[i] <= right_array[j]:
                array[k] = left_array[i]
                i += 1
            else:
                array[k] = right_array[j]
                j += 1
            k += 1
        
        while i < len(left_array):
            array[k] = left_array[i]
            i += 1
            k += 1
        
        while j < len(right_array):
            array[k] = right_array[j]
            j += 1
            k += 1

    def recursive(left: int, right: int) -> None:
        nonlocal array

        if left < right:
            mid = (left + right) // 2

            recursive(left, mid)
            recursive(mid + 1, right)
            merge(left, right)
    
    recursive(0, len(array) - 1)
```

Complexity analysis states that merge sort runs at O(n log n) in all cases. O(n) additional space, however, is used as auxiliary storage during the merge step.

Heap sort is a sorting technique that simply utilizes the heap data structure, whose capabilities can sort a list in O(n log n) time every time (and is easy to remember!) Additional space complexity will depend on the implementation of the heap. For instance, implementing a heap sort with python's `heapq` library will take O(n) auxiliary space:

```python
def heap_sort(array: list[X]) -> None:
    import heapq

    h = []
    for value in array:
        heappush(h, value)
    array = [heappop(h) for i in range(len(h))]
```

but by implementing an in-place function to heapify slices of a list, it can be done in O(1) additional space.

```python
def heap_sort_inplace(array: list[X]) -> None:
    def heapify_slice(left: int) -> None:
        ...
    
    for i in range(0, len(array)):
        heapify_slice(i)
```

Unfortunately, Python's `heapq` package does not play all that nicely with slices as it passes lists. Rewriting this package in order to use indices would work nicely and allow this to run with O(1) additional space complexity.

Other sorting algorithms and their complexities can be found [here](https://www.geeksforgeeks.org/analysis-of-different-sorting-techniques/).

## Key Problems

* [Two Sum](../code/1-TwoSum.md)