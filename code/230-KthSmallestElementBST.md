# Kth Smallest Element in a BST

From [LeetCode](https://leetcode.com/problems/kth-smallest-element-in-a-bst/):

> Given the `root` of a binary search tree, and an integer `k`, return *the `k`th smallest value (**1-indexed**) of all the values of the nodes in the tree*.

## Intuition

We can use an inorder traversal with a counter to solve this problem.

## Approach

We will use an inorder traversal with a counter `ctr` that is initially zero. On every `visit`, `ctr` will be raised. Once `ctr` is set to `k`, we will store that value and raise an exception to trap and return.

## Complexity

* Time complexity: O(n)
* Space complexity: O(1)

## Code

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def solution(root: TreeNode, k: int) -> int:
    ctr: int = 0
    res: int = None

    def visit(node: TreeNode) -> None:
        nonlocal ctr
        nonlocal k

        ctr += 1
        if ctr == k:
            nonlocal res
            res = node.val
            raise Exception()

    def inorder(root: TreeNode) -> None:
        if root is None:
            return

        nonlocal visit

        inorder(root.left)
        visit(root)
        inorder(root.right)
    
    try:
        inorder(root)
    except:
        pass

    return res
```