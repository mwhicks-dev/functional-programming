# Invert Binary Tree

From [LeetCode](https://leetcode.com/problems/invert-binary-tree/):

> Given the `root` of a binary tree, invert the tree, and return its *root*.

## Intuition

We can use any traversal besides an inorder traversal to invert the binary tree, as the subtrees will be changed by the visit, causing an inorder traversal to fail.

## Approach

We will use a postorder traversal where the `visit` function swaps the left and right subtrees of the node.

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Code

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def solution(root: TreeNode) -> TreeNode:
    def visit(node: TreeNode) -> None:
        (node.left, node.right) = (node.right, node.left)
    
    if root is not None:
        solution(root.left)
        solution(root.right)
        visit(root)

    return root
```