# Linked List Cycle

From [LeetCode](https://leetcode.com/problems/linked-list-cycle/description/):

> Given `head`, the head of a linked list, determine if the linked list has a cycle in it.
> 
> There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.
> 
> Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

## Intuition

This is a more literal use of pointers; we will have a "fast" and "slow" pointer, each starting at `head`. The fast pointer will increment twice per iteration as long as it can; the slow pointer will only increment once.

We will be able to tell that there is a loop if the fast pointer ever "catches up" to the slow pointer, as far as it can tell; this will be done by simply tracking the nodes that the slow pointer has traversed. To save space, we will take care of this by making the value of slow-visited nodes to `None`. If we wanted to keep the list in-place, we could just as well use a set to track this information.

Interestingly, the other solution - tracking the nodes visited across a single traversal - is also O(n), but it is closer to n, whereas this strategy is closer to n / 2.

## Complexity

* Time complexity: O(n)
* Space complexity: O(1)

## Code

```python
def solution(gead: Optional[ListNode]) -> bool:
    slow: Optional[ListNode] = head
    fast: Optional[ListNode] = head

    while slow is not None or fast is not None:
        if fast is not None:
            if fast.val is None:
                return True
            fast = fast.next
            if fast is not None:
                fast = fast.next
        if slow is not None:
            slow.val = None
            slow = slow.next
    
    return False
```