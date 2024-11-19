# Combination Sum

From [LeetCode](https://leetcode.com/problems/combination-sum/):

> Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of `candidates` where the chosen numbers sum to `target`*. You may return the combinations in any order.
> 
> The **frequency** of an element `x` is the number of times it occurs in the array.
>
> The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the 
> frequency of at least one of the chosen numbers is different.
> 
> The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

## Intuition

We can generate solutions by, starting from an empty list, selecting one number at a time and decreasing our target by the number that we added. This should be done via backtracking to save some headache.

## Approach

We will begin by sorting `candidates` so that we can easily stop our search tree generation when close to a solution.

Our initial partial will be an empty list, `[]`, and our entire target `target`. For each unique `candidate` in `candidates` that is less than or equal to `target`, we will extend our partial candidate with this `candidate` and recur with the extension and `target - candidate`.

We can hash our candidates by converting them to a string. To validate uniqueness, we need for our partials to be monotonically increasing, which we can do by either sorting it in the hash function or using a minimum index during backtracking such that we only select from indices greater than or equal to that minimum. The latter is a better invariant since sorting strategies may vary between stack.

## Complexity

- Time complexity: O(2^n)
- Space complexity: O(2^n)

## Code

```python
def solution(candidates: list[int], target: int) -> list[list[int]]:
    res: list[list[int]] = []

    candidates.sort()

    visited: set[int] = set()
    def backtracking(partial: list[int] = [], 
            hash_string: int = '',
            remaining: int = target, 
            idx: int = 0) -> None:
        nonlocal visited
        hv: int = hash(hash_string)
        if hv in visited:
            return
        visited.add(hv)

        if remaining == 0:
            nonlocal res
            res.append(partial)
        else:
            nonlocal candidates
            for i in range(idx, len(candidates)):
                candidate: int = candidates[i]
                if candidate > remaining:
                    break
                
                backtracking(
                    partial + [candidate],
                    hash_string + f'{candidate},',
                    remaining - candidate,
                    i
                )
    
    backtracking()

    return res
```

If this is hard for you to read and replicate, you can use a `Candidate` class like we did in [Subsets](./78-Subsets.md) and [Permutations](./46-Permutations.md).
