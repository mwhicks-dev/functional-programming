# Merge Two Sorted Lists

From [LeetCode](https://leetcode.com/problems/merge-two-sorted-lists/):

> You are given the heads of two sorted linked lists `list1` and `list2`.
> 
> Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.
> 
> Return *the head of the merged linked list*.

## Intuition

We can do this by simply traversing through the lists depending on which is smaller.

## Approach

This can be done iteratively and recursively.

For the iterative solution, we will simply create a dummy node, and add the smaller of the two lists at every step until they have both reached their end.

For the recursive solution, once we determine the smaller of the two lists, we will assign its next value to the merged version of that node's next node and the other list's head, and then return the head.

## Complexity

* Time complexity: O(n + m)
* Space complexity: O(1)

## Code

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def iterative(list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
    dummy: ListNode = ListNode()
    curr: ListNode = dummy

    while list1 is not None or list2 is not None:
        if list1 is None:
            curr.next = list2
            list2 = None
        elif list2 is None:
            curr.next = list1
            list1 = None
        elif list1.val < list2.val:
            curr.next = list1
            list1 = list1.next
        else:
            curr.next = list2
            list2 = list2.next
        curr = curr.next
    
    return dummy.next

def recursive(list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
    # base cases
    if list1 is None and list2 is None:
        return None
    elif list1 is None:
        return list2
    elif list2 is None:
        return list1
    
    # recursive cases
    res: ListNode = None
    if list1.val < list2.val:
        list1.next = recursive(list1.next, list2)
        res = list1
    else:
        list2.next = recursive(list1, list2.next)
        res = list2
    return res
```
