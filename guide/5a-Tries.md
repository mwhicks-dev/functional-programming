# Tries

A *trie*, or prefix tree, is a type of *n*-ary [tree](./4-Trees.md) used to locate specific kets from a space based on their prefix. These are commonly used for string lookup features and single-word autocomplete tools.

## Key Concepts

Since tries are just a specialization of an *n*-ary tree, there is not much for to cover conceptually. We can, however, implement a trie.

TrieNode:

```python
class TrieNode:
    def __init__(self, terminal: bool = False):
        self.children = {}
        self.terminal = terminal
```

This common trie node is used to contain lower-case English words, so we implement it using an *n*-ary tree with *n* = 26.

Trie:

```python
class Trie:
    def __init__(self):
        self.root = TrieNode()  # empty node
    
    def insert(self, word: str) -> None:
        curr: TrieNode = self.root
        for c in word:
            if curr.child[c] is None:
                curr.child[c] = TrieNode()
            curr = curr.child[c]
        curr.terminal = True
    
    def contains_word(self, word: str) -> bool:
        curr: TrieNode = self.root
        for c in word:
            if curr.child[c] is None:
                return False
            curr = curr.child[c]
        return curr.terminal

    def contains_prefix(self, word: str) -> bool:
        curr: TrieNode = self.root
        for c in word:
            if curr.child[c] is None:
                return False
            curr = curr.child[c]
        return True
    
    def remove_word(self, word: str) -> None:
        curr: TrieNode = self.root
        for c in word:
            if curr.child[c] is None:
                return
            curr = curr.child[c]
        curr.terminal = False
    
    def remove_prefix(self, word: str) -> None:
        curr: TrieNode = self.root
        for c in word[:-1]:
            if curr.child[c] is None:
                return
            curr = curr.child[c]
        curr.child[word[-1]] = None
```

## Key Algorithms

None!

## Problems

- [Design Add and Search Words Data Structure](../code/211-DesignAddSearchWordsDataStructure.md)
- [Word Search II](../code/212-WordSearchII.md)