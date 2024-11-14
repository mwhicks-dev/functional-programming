# Longest Substring Without Repeating Characters

From [LeetCode](https://leetcode.com/problems/longest-substring-without-repeating-characters/):

> Given a string `s`, find the length of the **longest substring** (contiguous non-empty sequence of characters within a string) without repeating characters.

## Intuition

This is a very transparent sliding window problem.

## Approach

We will begin with a sliding window that starts at `0` and has size `0`, the latter of which will be our initial result. From here, we will increase the size while tracking the number of each character, assigning our result whenever we find that there is no more than one of each character, and sliding the window when there is some repeated one. When the window can no longer grow nor slide, we will return the found result.

## Complexity

* Time complexity: O(n)
* Space complexity: O(26 + 26 + 10 + 1) = O(1)

## Code

```python
def solution(s: str) -> int:
    # create data structures to track important values
    character_map: dict[str, int] = {}
    frequency_map: dict[int, set[str]] = {}

    def increment(c: str) -> None:
        """
        Safely increment a character c in data structures
        """
        nonlocal character_map
        nonlocal frequency_map

        # preprocessing
        if c not in character_map:
            character_map[c] = 0
        else:
            initial_frequency: int = character_map[c]
            frequency_map[initial_frequency].remove(c)
            if len(frequency_map[initial_frequency]) == 0:
                del frequency_map[initial_frequency]
        
        # increase character frequency
        character_map[c] += 1
        
        # postprocessing
        frequency: int = character_map[c]
        if frequency not in frequency_map:
            frequency_map[frequency] = set()
        frequency_map[frequency].add(c)
    
    def decrement(c: str) -> None:
        """
        Safely decrement a character c in data structures
        """
        nonlocal character_map
        nonlocal frequency_map

        # preprocessing
        initial_frequency: int = character_map[c]
        frequency_map[initial_frequency].remove(c)
        if len(frequency_map[initial_frequency]) == 0:
            del frequency_map[initial_frequency]
        
        # decrease character frequency
        character_map[c] -= 1

        # postprocessing
        frequency: int = character_map[c]
        if frequency not in frequency_map:
            frequency_map[frequency] = set()
        frequency_map[frequency].add(c)

    (start, size, res) = (0, 0, 0)
    while start + size < len(s):
        increment(s[start + size])
        if max(frequency_map.keys()) <= 1:
            # increase size
            size += 1
            res = size

        else:
            # increase start
            decrement(s[start])
            start += 1
    
    return res
```

There is some room for optimization here, but this solution has the optimal time and space complexity and is easy to understand/describe. Once you have that, you can utilize a solution more similar to a classic two-pointers one that uses clever hashing to resize your window:

```python
def solution(s: str) -> int:
    last_index: dict[str, int] = {}

    (left, res) = (0, 0)
    for right in range(0, len(s)):
        # reset left s.t. no duplicate
        c = s[right]
        if c in last_index:
            left = max(left, last_index[c] + 1)
        
        # compute res
        res = max(res, right - left + 1)

        # update last index of c
        last_index[c] = right
    
    return res
```
