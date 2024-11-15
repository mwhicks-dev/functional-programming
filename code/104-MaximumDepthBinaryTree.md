# Maximum Depth of Binary Tree

From [LeetCode](https://leetcode.com/problems/maximum-depth-of-binary-tree/):

> Given the `root` of a binary tree, return *its maximum depth*.
> 
> A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

## Intuition

We can do this recursively by either
- keeping track of the maximum externally while traversing the tree; or
- propagating the depth up the tree and keeping track of the maximum.

## Approach

We will use the latter approach. Note that, in this case, the root has a depth of 1. Our recursive function will return:
- 0 if the node is `null`
- 1 + max{left subproblem, right subproblem} otherwise

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

def solution(root: TreeNode) -> int:
    if root is None:
        return 0
    
    return 1 + max(solution(root.left), solution(root.right))
```