# Linked List

## Key Concepts

Linked Lists, like arrays, are data structures used to store a sequence of objects. Unlike arrays, however, Linked Lists do not have a contiguous block of memory - instead, these lists are often represented as "nodes" that connect a list item to an adjacent list item or items (that is, the previous and next nodes). These are often implemented as references or pointers.

Due to the non-contiguous nature of linked lists, these lists do not have random access, a key weakness in cases where one must retrieve a value by index. Unlike array-based lists, however, items can be added and removed freely from the list once they *have* been accessed. Queues, for instance, which pop from the front and push to the back, are implemented using linked lists.

Linked Lists can be *singly linked*, with each node having a pointer to its next neighbor, or *doubly linked*, where each node having a pointer to both its next and previous neighbor. Furthermore, more formal Linked List data structures often use a "dummy node" containing no data to be a permanent head of the list, even if all nodes are removed.

## Key Algorithms

### Singly Linked List

List Node:

```python
from typing import Any

class ListNode:

    def __init__(self, data: Any = None):
        self.data = data
        self.next = None
```

Linked List:

```python
from typing import Any

class LinkedList:

    def __init__(self):
        self.head = ListNode()
        self.size = 0
    
    def insert(self, index: int, data) -> None:
        if index < 0 or index > self.size:
            raise Exception()
        
        pos: int = 0
        curr: ListNode = self.head

        while pos < index:
            curr = curr.next
            pos += 1
        
        node: ListNode = ListNode(data)
        node.next = curr.next
        curr.next = node

        self.size += 1
    
    def remove(self, index: int) -> Any:
        if index < 0 or index >= self.size:
            raise Exception()
        
        pos: int = 0
        curr: ListNode = self.head

        while pos < index:
            curr = curr.next
            pos += 1
        
        node: ListNode = curr.next
        curr.next = node.next

        self.size -= 1

        return node.data
    
    def get(self, index: int) -> Any:
        if index < 0 or index >= self.size:
            raise Exception()
        
        pos: int = 0
        curr: ListNode = self.head

        while pos < index:
            curr = curr.next
            pos += 1
        
        res = curr.next.data

        return res
```

### Doubly Linked List

List Node:

```python
from typing import Any

class ListNode:

    def __init__(self, data: Any = None):
        self.data = data
        self.next = None
        self.prev = None
```

Linked List:

```python
from typing import Any

class LinkedList:

    def __init__(self):
        self.head = ListNode()
        self.size = 0
    
    def insert(self, index: int, data) -> None:
        if index < 0 or index > self.size:
            raise Exception()
        
        pos: int = 0
        curr: ListNode = self.head

        while pos < index:
            curr = curr.next
            pos += 1
        
        node: ListNode = ListNode(data)
        node.prev = curr
        node.next = curr.next

        node.prev.next = node
        node.next.prev = node

        self.size += 1
    
    def remove(self, index: int) -> Any:
        if index < 0 or index >= self.size:
            raise Exception()
        
        pos: int = 0
        curr: ListNode = self.head

        while pos < index:
            curr = curr.next
            pos += 1
        
        node: ListNode = curr.next

        node.prev.next = node.next
        node.next.prev = node.prev

        self.size -= 1

        return node.data
    
    def get(self, index: int) -> Any:
        if index < 0 or index >= self.size:
            raise Exception()
        
        pos: int = 0
        curr: ListNode = self.head

        while pos < index:
            curr = curr.next
            pos += 1
        
        res = curr.next.data

        return res
```

### Reversal of a Linked List

This can be done both iteratively and recursively, but the latter seems to run faster most of the time.

```python
def iterative(head: ListNode) -> ListNode:
    prev = None
    curr = head

    while curr is not None:
        tmp = curr.next
        curr.next = prev
        prev = curr
        curr = tmp
    
    return prev

def recursive(head: ListNode) -> ListNode:
    # base case
    if head is None or head.next is None:
        return head
    
    # recursive case
    res: ListNode = recursive(head.next)  # retrieve tail

    head.next.next = head
    head.next = None

    return res
```

## Problems

* [Merge Two Sorted Lists](../code/21-MergeTwoSortedLists.md)
* [Reorder List](../code/143-ReorderList.md)
* [Linked List Cycle](../code/141-LinkedListCycle.md)
* [LRU Cache](../code/146-LRUCache.md)
