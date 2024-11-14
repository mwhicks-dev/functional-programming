# LRU Cache

From [LeetCode](https://leetcode.com/problems/lru-cache/):

> Design a data structure that follows the constraints of a [Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU).
> 
> Implement the `LRUCache` class:
> 
> - `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
> - `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
> - `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.
> 
> The functions `get` and `put` must each run in `O(1)` average time complexity.

## Intuition

There are two functional data structures in the LRU Cache problem: The simple underlying map, which works as any plain map or dictionary, and LRU tracker which is updated upon any `put` or `get` to the `LRUCache`.

The intuitive approach is to maintain a stack of least recently used keys, popping them as necessary in order to determine a key to remove. However, this raises a concern, as edge cases will cause certain runtimes to be O(m) where m is the number of operations.

Instead, we would like to LRU tracker to contain unique, singular identifiers for cache members which can be accessed, added, and moved in constant time. This can be done using a *positionally linked list*, or rather, an implementation of the doubly linked list that favors tracking of data not by index, but simply by its position (which is returned when a new datum is added). You have likely seen this data structure before if you took a Data Structures & Algorithms class for which you programmed in Java, but the positional abstract data type is not found in other programming languages explicitly because it doesn't add much.

## Approach

We will implement a `PositionalLinkedList` data structure specialized for our problem with the following stubs:

```python
class Position:
    prev: Position
    next: Position

class PositionalLinkedList:
    head: Position
    tail: Position
    data: dict[Position, int]
    size: int
    
    def insert(self, data: int) -> Position:
        """
        Insert a new data point
        """
        pass

    def move_to_front(self, position: Position) -> None:
        """
        Takes the passed position and moves it to the front of the list.
        """
        pass
    
    def pop_from_back(self) -> int:
        """
        Removes the element behind tail, and returns its stored data
        """
        pass
    
    def __len__(self) -> int:
        return self.size
```

We are not encoding any information surrounding the capacity nor key-value things into this data structure because the PLL does not need to know about it. Instead, that higher-level information will be left to the `LRUCache` (and should be really simple).

This `PositionalLinkedList` will allow for `put` and `get` operations to impact the usage tracking in O(1) time and O(capacity) space.

## Complexity

- Time complexity: O(1) average-case get and put
- Space complexity: O(capacity)

## Code

```python
class Position:
    def __init__(self):
        self.prev: Position = None
        self.next: Position = None

class PositionalLinkedList:
    def __init__(self):
        self.head: Position = Position()
        self.tail: Position = Position()
        self.data: dict[Position, int] = {}
        self.size: int = 0

        self.head.next = self.tail
        self.tail.prev = self.head
    
    def insert(self, data: int) -> Position:
        """
        Insert a new data point
        """
        res: Position = Position()
        self.data[res] = data
        self.size += 1

        self.move_to_front(res)

        return res

    def move_to_front(self, position: Position) -> None:
        """
        Takes the passed position and moves it to the front of the list.
        """
        # remove from old neighbors
        if position.prev is not None:
            position.prev.next = position.next
        if position.next is not None:
            position.next.prev = position.prev
        
        # (re)insert between head and head.next
        position.next = self.head.next
        position.prev = self.head
        position.prev.next = position
        position.next.prev = position
    
    def pop_from_back(self) -> int:
        """
        Removes the element behind tail, and returns its stored data
        """
        if self.size == 0:
            raise Exception()
        
        position: Position = self.tail.prev

        # remove from old neighbors
        if position.prev is not None:
            position.prev.next = position.next
        if position.next is not None:
            position.next.prev = position.prev
        
        res: int = self.data[position]
        del self.data[position]

        self.size -= 1

        return res
    
    def __len__(self) -> int:
        return self.size

class LRUCache:

    def __init__(self, capacity: int):
        self.capacity: int = capacity
        self.pll: PositionalLinkedList = PositionalLinkedList()
        self.data: dict[int, tuple[int, Position]] = {}

    def get(self, key: int) -> int:
        if key not in self.data:
            return -1
        (value, pos) = self.data[key]
        self.pll.move_to_front(pos)
        return value

    def put(self, key: int, value: int) -> None:
        pos: Position = self.data[key][1] if key in self.data else self.pll.insert(key)
        self.pll.move_to_front(pos)
        self.data[key] = (value, pos)

        if len(self.pll) > self.capacity:
            del self.data[self.pll.pop_from_back()]
```

Like usual, with these OOP-like solutions, things can typically be sped up by relying on purely functional programming.
