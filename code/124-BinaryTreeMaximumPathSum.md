# Binary Tree Maximum Path Sum

From [LeetCode](https://leetcode.com/problems/binary-tree-maximum-path-sum/):

> A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.
> 
> The **path sum** of a path is the sum of the node's values in the path.
> 
> Given the `root` of a binary tree, return the *maximum **path sum** of any **non-empty** path*.

## Intuition

Specifying some terminology: We will call the *downwards path sum* a special type of path sum that only goes downwards from its highest node.

We want to find the best path at every node, and can do this recursively from the bottom up (the best possible path at a leaf is just the leaf).

## Approach

We can use a postorder traversal to, at any node, find the maximum downwards path possible sum from its children, and then use that to compute: (1) the max path at the node, and (2) the max downwards path sum starting at the node. The latter will be returned for use in the parent problem.

The max path at a node may be any of the following:
1. The node itself.
1. The max downwards path sum starting at the node.
1. The node plus the child downward path sums.

## Complexity

* Time complexity: O(n)
* Space complexity: O(1)

## Code

```python
def solution(root: TreeNode) -> int:
    res: int = -1000 # boundary value from LC
    
    def postorder(node: TreeNode) -> int:
        if node is None:
            return -1000  # boundary value from LC
        
        (left, right) = (postorder(node.left), postorder(node.right))

        nonlocal res
        res = max(
            res, 
            node.val, 
            node.val + left, 
            node.val + right, 
            node.val + left + right
        )

        return max(
            node.val,
            node.val + left,
            node.val + right
        )
    
    postorder(root)

    return res
```
