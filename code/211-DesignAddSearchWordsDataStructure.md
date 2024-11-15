From [LeetCode](https://leetcode.com/problems/design-add-and-search-words-data-structure/):

> Design a data structure that supports adding new words and finding if a string matches any previously added string.
> 
> Implement the `WordDictionary` class:
> 
> `WordDictionary()` Initializes the object.
> - `void addWord(word)` Adds `word` to the data structure, it can be matched later.
> - `bool search(word)` Returns `true` if there is any string in the data structure that matches `word` or `false` otherwise. word may contain dots `'.'` where dots can be matched with any letter.

## Intuition

This is a straightforward [trie](../guide/5a-Tries.md) problem.

## Approach

For this problem, we will implement a mostly basic trie with recursive serach and, for a `'.'`, recur over each of the children.

## Complexity

Let *l* be the length of a word, and let *n* be the number of words in a test case...

- Time complexity: O(*l*) for each function call
- Time complexity: O(*ln*) for data structure

## Code

```python
class TrieNode:
    def __init__(self):
        self.terminal: bool = False
        self.children: dict[str, TrieNode] = {}

class WordDictionary:
    def __init__(self):
        self.root: TrieNode = TrieNode()
    
    def addWord(self, word: str) -> None:
        curr: TrieNode = self.root
        for c in word:
            if c not in curr.children:
                curr.children[c] = TrieNode()
            curr = curr.children[c]
        curr.terminal = True
    
    def search(self, word: str) -> bool:
        cstr: list[str] = list(reversed(word))  # reversed for O(1) pops

        def recursive(node: TrieNode, stack: list[str]) -> bool:
            # base cases
            if node is None:
                return False
            if len(stack) == 0:
                return node.terminal
            
            # recursive cases
            c = stack[-1]
            del stack[-1]

            if c == '.':
                for potential_match in node.children:
                    stack_copy = stack.copy()
                    if recursive(node.children[potential_match], stack_copy):
                        return True
            else:
                if recursive(node.children.get(c, None), stack):
                    return True
            
            return False
        
        return recursive(self.root, cstr)
```
