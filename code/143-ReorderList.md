# Reorder List

From [LeetCode](https://leetcode.com/problems/reorder-list/):

> You are given the head of a singly linked-list. The list can be represented as:
> 
> > L_0 > L_1 > ... > L_{n-1} > L_n
> 
> *Reorder the list to be on the following form*:
> 
> > L_0 > L_n > L_1 > L_{n-1} > L_2 > L_{n-2} > ...
> 
> You may not modify the values in the list's nodes. Only nodes themselves may be changed.

## Intuition

A slow, but very slick, approach to this problem is to, at every node, reverse it starting at its next node, and point the current node to the returned head of the subproblem. Unfortunately, this runs in O(n log n) time and thus is not optimal. However, list reversal can still be used - this problem decomposes into merging the front half of the lift and the (reversed) back half for the same reason.

## Approach

There are two O(n) strategies for this problem: a Two Pointers solution and a hacky array solution.

1. The Two Pointers solution involves having a "fast" and "slow" pointer, similar to [Linked List Cycle](./141-LinkedListCycle.md), to find the center of the list. From there, the second half of the list should be reversed, and the two lists should be merged, alternating between the first and second list.
1. Add the nodes into an array, reorder them using random access, and then fix the pointers.

We will not be implementing the latter here, but you can try on your own.

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Code

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def solution(head: Optional[ListNode]) -> None:
    if head is None or head.next is None:
        return head
    
    # find center using two pointers
    (slow_prev, slow, fast) = (None, head, head)
    iteration: int = 0
    while fast is not None:
        fast = fast.next
        if iteration % 2 == 1:
            slow_prev = slow
            slow = slow.next
        iteration += 1
    
    # separate lists
    slow_prev.next = None

    # reverse at slow
    (prev, curr) = (None, slow)
    while curr is not None:
        tmp = curr.next
        curr.next = prev
        prev = curr
        curr = tmp
    
    head1: ListNode = head
    head2: ListNode = prev


    # splice head1 -> head2 -> head1.next -> ...
    dummy: ListNode = ListNode()
    curr: ListNode = dummy
    while head1 is not None or head2 is not None:
        if head1 is not None:
            curr.next = head1
            head1 = head1.next
            curr = curr.next
        if head2 is not None:
            curr.next = head2
            head2 = head2.next
            curr = curr.next
    
    return dummy.next
```