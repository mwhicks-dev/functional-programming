# Largest Rectangle in Histogram

From [LeetCode](https://leetcode.com/problems/largest-rectangle-in-histogram):

> Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return *the area of the largest rectangle in the histogram*.

**Example 1:**

![`heights` = [2, 1, 5, 6, 3]; ans 10](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)

## Intuition

We can form potential solutions by starting at any one value and "extending" it to the right so long as the next right rectangle is of a larger size. These larger rectangles cannot be extended left, so doing this one-directional extension works.

Furthermore, when we do find a smaller rectangle to the right of a current position, we can also look backwards at that point in order to see how far left that rectangle could extend. This will, of course, also restart the rectangle sweeping to the right.

By keeping track of the current rectangle being extended to the right and verifying left-sweeping rectangles on a need-to-know basis, we will be able to check all feasible solutions.

## Approach

We can use a *monotonically increasing stack*, that is, a stack whose members are always increasing in value or remaining the same, to implement this strategy. By enforcing this monotonic rule, we can freely add increasing-height members to the stack, but when this is no longer the case, must remove the larger elements to its left (at which point we can find the area of the popped rectangle).

Our stack will also include the index of each height, for which the monotonic rule will be maintained by nature. This will allow us to compute areas quickly after members are popped.

## Complexity

* Time complexity: O(n^2)
* Space complexity: O(n)

## Code

```python
def solution(heights: list[int]) -> int:
    def squarea(start: int, end: int, height: int) -> int:
        return height * (end - start + 1)

    monotonic: list[tuple[int, int]] = []  # (index, height)

    res: int = 0

    # iterate over list and generate stack
    for index in range(0, len(heights)):
        height = heights[index]

        # pop all values for which height would break monotonic rule
        (start, end, min_height) = (None, None, None)
        while len(monotonic) > 0 and height < monotonic[-1][1]:
            (popped_index, popped_height) = monotonic[-1]
            if start is None or popped_index < start:
                start = popped_index
            if end is None or popped_index > end:
                end = popped_index
            if min_height is None or popped_height < min_height:
                min_height = popped_height
            
            # check for maximum area within popped rectangle so far
            area = squarea(start, end, min_height)
            if area > res:
                res = area

            del monotonic[-1]
        
        # add best usable rectangle height back to stack at start index
        if start is not None and min_height is not None:
            monotonic.append((start, height))
        
        # push current pair to monotonic stack
        monotonic.append((index, height))

    # now, empty stack
    (start, end, min_height) = (None, None, None)
    while len(monotonic) > 0:
        (popped_index, popped_height) = monotonic[-1]

        if start is None or popped_index < start:
            start = popped_index
        if end is None or popped_index > end:
            end = popped_index
        if min_height is None or popped_height < min_height:
            min_height = popped_height
        
        area = squarea(start, end, min_height)
        if area > res:
            res = area

        del monotonic[-1]

    return res
```

To reduce some duplicate code, I would opt to use a strategy pattern for emptying the stack. This pattern would look like:

```python
class MonotonicPopStrategy(ABC):

    @abstractmethod
    def condition(self, stack: list[tuple[int, int]]) -> bool:
        """
        Indicates whether or not to continue emptying the stack.
        """
        pass
    
    def empty(self, stack: list[tuple[int, int]]) -> int:
        """
        Empties the stack while `condition` evaluates to `True`.

        Returns the best possible area found while emptying.
        """
        res: int = 0

        (start, end, min_height) = (None, None, None)
        while len(stack) > 0 and self.condition(stack):
            (popped_index, popped_height) = stack[-1]
            del stack[-1]

            if start is None or popped_index < start:
                start = popped_index
            if end is None or popped_index > end:
                end = popped_index
            if min_height is None or popped_height < min_height:
                min_height = popped_height
            
            area = min_height * (end - start + 1)
            if area > res:
                res = area
        
        if start is not None and min_height is not None:
            stack.append((start, min_height))
        
        return res
```

[Here](https://leetcode.com/problems/largest-rectangle-in-histogram/solutions/6044567/oop-solution-with-monotonic-stack/) is a link to such a solution. This runs slower than the one above, but is a good example of using SOLID design/dependency inversion for a complex problem (besides LSP which is violated, though this can be solved easily).

This general solution strategy can be refined to [this 10-line functional form](https://leetcode.com/problems/largest-rectangle-in-histogram/solutions/5896035/10-lines-monotonic-stack-one-pass-dummy-node-technique) using a dummy height at the start and is worth taking a read over.
