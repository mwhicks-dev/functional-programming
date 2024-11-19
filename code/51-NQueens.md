# N-Queens

From [LeetCode](https://leetcode.com/problems/n-queens/):

> The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.
> 
> Given an integer `n`, return *all distinct solutions to the **n-queens puzzle***. You may return the answer in **any order**.
> 
> Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

## Intuition

The N-Queens puzzle is a classic backtracking problem, but one could arrive at that intuitively by trying to work through it by hand - you solve this problem by adding one queen at a time to the board and each time taking away what squares to use.

## Approach

Our partial candidates will be defined by positions of the queens on the board, which we will encode in a 2D character array (`'Q'` indicating queen placement, `''` indicating available square, and `'.'` indicating unavailable square).

## Complexity

- Time complexity: O(N^2 * N!)
- Space complexity: O(N^3)

## Code

Because of the more complicated code, I think that N-Queens works better with a Candidate class to represent partial candidates. Generally I feel this way about all large problems, but that is because I spent much more time on OOP than functional programming.

```python
class Candidate:
    def __init__(self, 
            queens=[], 
            board: list[str]=[]) -> None:
        self.queens = queens
        self.board = board

        self.n = len(board)
    
    def solution(self) -> bool:
        return len(self.queens) == self.n
    
    def __hash__(self) -> int:
        return hash(''.join(self.board))
    
    @staticmethod
    def place_queen(queen: tuple[int, int], 
            queens: list[tuple[int, int]],
            board: list[str]) -> None:
        queens.append(queen)

        (i, j) = queen
        
        board[i] = board[i][0:j] + 'Q' + board[i][j+1:len(board)]

    def extend(self, pos: tuple[int, int]):
        candidate: Candidate = Candidate(
            queens=self.queens.copy(),
            board=self.board.copy()
        )
        Candidate.place_queen(pos, candidate.queens, candidate.board)
        
        return candidate
```

```python
def solution(self, n: int) -> list[list[str]]:
    res: list[list[str]] = []

    visited: set[int] = set()
    def backtracking(candidate: Candidate) -> None:
        nonlocal visited
        candidate.queens.sort(key=lambda x: x[0])  # sorts queens by first index
        hv: int = hash(candidate)
        if hv in visited:
            return
        visited.add(hv)
        
        if candidate.solution():
            res.append(candidate.board)
        else:
            nonlocal n

            low: int = 0 if len(candidate.queens) == 0 else candidate.queens[-1][0] + 1
            for i in range(low, n):
                for j in range(0, n):
                    # check for collisions
                    collision: bool = False
                    for (row, col) in candidate.queens:
                        if col == j or abs(row - i) == abs(col - j):
                            collision = True
                            break

                    if not collision:
                        backtracking(candidate.extend((i, j)))

    initial_board: list[str] = ["." * n for i in range(n)]
    backtracking(Candidate(board=initial_board))

    return res
```

N-Queens is probably one of the most popular programming problems of all time, besides some NP-hards and other similarly famous ones, so it has been extremely optimized. This solution only beats 5% of all of the ones submitted to LC! Good enhancements include keeping track of which squares are still available and updating them on placement so that you can tell earlier when a partial solution is infeasible.
