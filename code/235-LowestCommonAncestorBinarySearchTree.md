# Lowest Common Ancestor of a Binary Search Tree

From [LeetCode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/):

> Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.
> 
> According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): "The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**)."

## Intuition

Assume that `p` and `q` are both in the tree. Then we know, by default, that the root is a common ancestor. We can use the values of the current node (starting with the `root`) and its children to make some assumptions about whether or not they can contain `p` and `q`.

## Approach

Let `node` be the current node. Since `node` is the root of a BST, we know that `node.left` only contains values less than `node.val`, and that `node.right` only contains values greater than `node.val`.

We can traverse down the list until `node.val` is between (inclusive) `p.val` and `q.val` by moving to the left or right child.

## Complexity

- Time complexity: O(n) worst case, with O(log n) average case
- Space complexity: O(1)

## Code

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

def solution(root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
    (low, high) = (p.val, q.val) if p.val < q.val else (q.val, p.val)

    while not (root.val >= low and root.val <= high):
        if root.val > high:
            root = root.left
        else:
            root = root.right
    
    return root
```