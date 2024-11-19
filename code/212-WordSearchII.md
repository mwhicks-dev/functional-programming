# Word Search II

From [LeetCode](https://leetcode.com/problems/word-search-ii/):

> Given an `m x n` `board` of characters and a list of strings `words`, return *all words on the board*.
> 
> Each word must be constructed from letters of sequentially adjacent cells, where **adjacent cells** are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

## Intuition

Like [Word Search](./79-WordSearch.md), we will begin by constructing a [trie](../guide/5a-Tries.md) that indexes all of `words` in order to make the search more practical. However, this time instead of performing a simple search-based strategy, we will also use [backtracking](../guide/5b-Backtracking.md) to save memory; in general, any time where different instances of the problem must maintain different instances of the visited nodes, backtracking tends to outperform generic search algorithms due to its utilization of memory.

## Approach

We will create a simple trie using hashing that encodes words. Then, starting from an empty partial candidate, we will enumerate an "initial" partial candidate beginning at each tile. Found words will be added to a result set which will be listified before returning.

## Complexity

- Time complexity: O(mn!)
- Space complexity: O(mn!)

## Code

TrieNode:

```python
class TrieNode:
    def __init__(self):
        self.terminal = False
        self.word = None
        self.children = {}
```

Trie:

```python
class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word: str) -> None:
        curr: TrieNode = self.root
        for c in word:
            if c not in curr.children:
                curr.children[c] = TrieNode()
            curr = curr.children[c]
        curr.word = word
        curr.terminal = True
```

Since search will be done internally by our partial candidates, we need not handle this ourselves.

Candidate:

```python
class Candidate:
    def __init__(self, curr: TrieNode):
        self.visited = set()
        self.curr = curr
        self.prev = None
    
    def solution(self) -> bool:
        return self.curr.terminal if self.feasible() else False
    
    def feasible(self) -> bool:
        return self.curr is not None
    
    def extend(self, c: str, prev: tuple[int, int]) -> None:
        self.visited.add(prev)
        self.prev = prev
        self.curr = self.curr.children.get(c, None)
    
    def clone(self):
        cp: Candidate = Candidate(self.curr)
        cp.visited = self.visited.copy()

        return cp
```

Solution:

```python
def solution(board: list[list[str]], words: list[str]) -> list[str]:
    res: set[str] = set()

    (rows, cols) = (len(board), len(board[0]))

    trie: Trie = Trie()
    for word in words:
        trie.insert(word)

    def backtracking(partial: Candidate) -> None:
        if partial.solution():
            nonlocal res
            res.add(partial.curr.word)
        if partial.feasible():
            nonlocal rows
            nonlocal cols

            (i, j) = partial.prev
            neighbors: list[tuple[int, int]] = [
                (i + 1, j),
                (i - 1, j),
                (i, j + 1),
                (i, j - 1)
            ]

            for neighbor in neighbors:
                if neighbor in partial.visited:
                    continue
                if neighbor[0] < 0 or neighbor[1] < 0:
                    continue
                if neighbor[0] >= rows or neighbor[1] >= cols:
                    continue
                
                nonlocal board

                next_candidate: Candidate = partial.clone()
                c: str = board[neighbor[0]][neighbor[1]]
                next_candidate.extend(c, neighbor)

                backtracking(next_candidate)
    
    for i in range(rows):
        for j in range(cols):
            initial: Candidate = Candidate(trie.root)
            initial.extend(board[i][j], (i, j))
            backtracking(initial)

    return list(res)
```

An interesting scenario where you keep running after a solution is found so long as the partial candidate is still feasible.

This actually does not run on LC because some optimizations are required for really annoying test cases. I do not care to do said optimizations as this core loop of the problem is much, much more important to understand.
