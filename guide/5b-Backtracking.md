# Backtracking

*Backtracking* algorithms are a type of algorithm that incrementally build solutions in an *n*-ary tree structure, one step at a time, until either a solution is found or no solution can be found, at which point it will return to the most recent feasible option.

## Key Concepts

Backtracking algorithms use what is called a *partial candidate*, a partial solution that could be completed in various ways. At any given iteration of a backtracking algorithm, one extension is added to the partial candidate, called a *candidate extension step*. This process is continued until a (candidate) solution is found, or until there is no longer a possible solution. Either way, we will then regress to the previous partial candidate.

This process traverses a theoretical "potential search tree" *P* that contains each partial candidate and solution. At each node *c* of *P*, the algorithm will determine whether or not a potential solution is possible; if not, then it will not continue searching *c* at all.

## Key Algorithms

Here is a generic form of the backtracking algorithm in Python. Let *Candidate* be a class representing partial candidates and candidate solutions.

```python
candidate_solutions: list[Candidate] = []

visited: set[int] = set()
def backtracking(partial: Candidate) -> None:
    nonlocal visited
    hv: int = hash(partial)
    if hash(hv) in visited:
        return
    visited.add(hv)

    if solution(partial):
        nonlocal candidate_solutions
        candidate_solutions.append(partial)
    elif infeasible(partial):
        return
    else:
        for candidate in extend(partial):
            backtracking(candidate)
```

The function `solution` verifies whether `partial` is a candidate solution, `infeasible` determines if a solution is even possible, and `extend` generates all possible candidate extensions.

Though it seems as if `infeasible` could be wrapped into `extend`, they do different things. `infeasible` checks if it can determine early on in the search process that there is not a possible solution - `extend` performs an increment to the partial solution passed.

## Problems

* [Subsets](../code/78-Subsets.md)
* [Permutations](../code/46-Permutations.md)
* [Combination Sum](../code/39-CombinationSum.md)
* [Word Search](../code/79-WordSearch.md)
* [Word Search II](../code/212-WordSearchII.md)
* [N Queens](../code/51-NQueens.md)
