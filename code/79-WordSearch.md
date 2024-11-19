# Word Search

From [LeetCode](https://leetcode.com/problems/word-search/):

> Given an `m x n` grid of characters `board` and a string `word`, return I.
> 
> The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

## Intuition

In this problem, we want to search a 2D grid for a word, which sounds easy on its own but causes some challenges when considering that we need to maintain several different instances of what vertices we have visited. 

We will use backtracking in order to save some memory during this process, as it allows us to just generate one chain of partial solutions at a time.

## Approach

We will begin by doing some preprocessing our board to, for each character, get a set of coordinates that we will want to look up later.

Our partial candidates will simply be composed of a set of coordinates, each representing one character in the target word. This, of course, will initially be empty. We will also pass the previous coordinate, initially `null`.

During each iteration of backtracking, we will use the number of coordinates along that we are in order to determine what character we are looking for in O(1) time. We can utilize our coordinate map alongside a list of potential neighbors of our last coordinate (if not `null`) to find our extensions.

If we ever find a match for `word`, we can raise an exception to trap and return after marking our output to `true`.

## Complexity

- Time complexity: O(mn)
- Space complexity: O(mn * l)

## Code

```python
def solution(board: list[list[str]], word: str) -> bool:
    res: bool = False

    loc: dict[str, set[tuple[int, int]]] = {}
    for i in range(len(board)):
        for j in range(len(board[i])):
            c: str = board[i][j]
            if c not in loc:
                loc[c] = set()
            loc[c].add((i, j))
    
    def backtracking(visited: set[tuple[int, int]] = set(), 
            prev: tuple[int, int] = None) -> None:
        nonlocal word
        if len(visited) == len(word):
            nonlocal res
            res = True
            raise Exception()
        target: str = word[len(visited)]

        nonlocal loc
        candidates: set[tuple[int, int]] = None
        if prev is None:
            candidates = loc[target]
        else:
            candidates = set()
            (i, j) = prev
            for candidate in [(i - 1, j), (i + 1, j), (i, j - 1), (i, j + 1)]:
                if candidate in loc[target]:
                    candidates.add(candidate)
        
        for candidate in candidates:
            if candidate in visited:
                continue
                
            partial_visited: set[tuple[int, int]] = visited.copy()
            partial_visited.add(candidate)

            partial_prev: tuple[int, int] = candidate

            backtracking(partial_visited, partial_prev)
    
    try:
        backtracking()
    except:
        pass

    return res
```
