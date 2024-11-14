# Longest Repeating Character Replacement

From [LeetCode](https://leetcode.com/problems/longest-repeating-character-replacement/):

> You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.
> 
> Return the *length of the longest substring containing the same letter you can get after performing the above operations*.

## Intuition

Longest substring satisfying condition is an indicator of a sliding window problem. Since we can replace any *k* characters, what we really need is the longest substring of length *l* whose greatest character frequency is *l - k* (as the other k) can be replaced.

## Approach

We will create a sliding window with an initial start at `0` and an intial size of `k`, and use a frequency map `m` to keep track of character frequencies. Our result will be the largest value for `size` at which `max(m.values()) >= size - k`.

## Complexity

* Time complexity: O(n)
* Space complexity: O(26) = O(1)

## Code

```python
def solution(s: str, k: int) -> int:
    m: dict[str, int] = {'#': 0}

    def increment(c: str) -> None:
        if c not in m:
            m[c] = 0
        m[c] += 1
    
    def decrement(c: str) -> None:
        m[c] -= 1

    (start, size, res) = (0, 0, 0)
    while start + size <= len(s):
        if size > 0:
            increment(s[start + size - 1])
        
        if max(0, m.values()) >= size - k:
            res = size
            size += 1
        else:
            decrement(s[start])
            start += 1
    
    return res
```
