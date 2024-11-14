# Sliding Window Maximum

From [LeetCode](https://leetcode.com/problems/sliding-window-maximum/):

> You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.
> 
> Return *the max sliding window*.

## Intuition

On this portion, the sliding window is the easy part (as long as you've seen one before), with the more challenging part being the implementation of data structures to keep track of the maximum values in O(1) time.

## Approach

We can implement a max queue that, at any given time, is able to determine what the maximum enqueued value is in O(1) time. Here is the relevant information we need to consider:

* Suppose that *a* is currently the maximum enqueued value, but a value *b > a* is enqueued. In such a case, *a* will be unable to be the maximum value until *b* is dequeued and can be discarded.
* Suppose that *a* is currently the maximum value, and is then enqueued again within the next *k* iterations. Then we must ensure that *a* is still the maximum after the first instance is dequeued.

With these considerations, we can implement our maximum queue using a [monotonically decreasing deque](../guide/2b-Stack.md). Until dequeued from the front, new items will be pushed to the back with a monotonically decreasing rule strictly enforced. When a value is popped from the stack, if it is the maximum at the front, it will be popped from the left.

## Complexity

- Time Complexity: O(n)
- Space Complexity: O(k)

## Code

Max Queue:

```python
from collections import deque

class MaxQueue:

    queue: deque[int]
    monotonic: deque[int]

    def __init__(self):
        self.queue = deque()
        self.monotonic = deque()
    
    def enqueue(self, val: int) -> None:
        self.queue.append(val)
        while len(self.monotonic) > 0 and self.monotonic[-1] < val:
            self.monotonic.pop()
        self.monotonic.append(val)
    
    def dequeue(self) -> None:
        val: int = self.queue.popleft()
        if self.max() == val:
            self.monotonic.popleft()
    
    def max(self) -> int:
        return self.monotonic[0]
```

Solution:

```python
def solution(nums: list[int], k: int) -> list[int]:
    res: list[int] = []
    queue: MaxQueue = MaxQueue()

    for i in range(k):
        queue.enqueue(nums[i])
    res.append(queue.max())
    
    for i in range(k, len(nums)):
        queue.dequeue()
        queue.enqueue(nums[i])
        res.append(queue.max())
    
    return res
```
