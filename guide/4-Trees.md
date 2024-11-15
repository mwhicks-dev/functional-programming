# Trees

*Trees*, similar to [Linked Lists](./3c-LinkedList.md), are a collection of individual data nodes connected to each other by references or pointers. Unlike a linked list, trees follow a non-linear hieararchy - each node has a parent and/or children.

## Key Concepts

We will begin by enumerating a binary, ternary, and *n*-ary tree, that is, a tree having 2, 3, and *n* maximum children per node:

```python
from typing import Any

class TreeNode:
    parent: TreeNode
    value: Any

class BinaryTreeNode(TreeNode):
    left: BinaryTreeNode
    right: BinaryTreeNode

class TernaryTreeNode(TreeNode):
    left: TernaryTreeNode
    middle: TernaryTreeNode
    right: TernaryTreeNode

class CapacityTreeNode(TreeNode):
    capacity: int
    children: set[CapacityTreeNode]

    def __init__(self, n: int):
        self.children = []
        self.capacity = n
    
    def num_children(self) -> int:
        return len(children)

    def add_child(self, node: CapacityTreeNode) -> None:
        if node in self.children or self.num_children() > self.capacity:
            raise Exception()
        self.children.add(node)
```

### Binary Trees

Some important terms:

* The *root* is the highest-level element of a (binary) tree.
* A *leaf* is a lowest-level node of a (binary) tree which has no children.
* A node's *ancestors* include any nodes between itself and the root of the tree.
* The *depth* of a tree node is the number of links required to reach the node from its ancestor root. For instance, the root has a depth of 0.

There are some additional classifiers for binary trees which we will itemize now.

* A *full binary tree* is a binary tree in which every node has exactly *0* or *2* children. (That is, no nodes have 1 child.)
* A *degenerate binary tree* is a binary tree in which every node has exactly *0* or *1* child. This special case functions like a LinkedList.
* A *balanced binary tree* is a binary tree for which the depth of any two leaves has a distance no greater than 1.
* A *perfect binary tree* is a balanced binary tree for which all inner nodes have two children and all leaves have the exact same depth.
* A *complete binary tree* is a balanced binary tree where all levels of the tree are completely filled except the bottom level, which has at least one node and is populated from left to right with no missing leaves in the layer.

A *binary search tree* is a binary tree that maintains the following invariants:
1. The left subtree of a BST node contains only nodes with values lesser than the node's value.
1. The right subtree of a BST node contains only nodes with values greater than the node's value.
1. The left and right subtree of a BST node is also a BST.

```python
from abc import ABC, abstractmethod
from typing import Any

def Comparable:
    @abstractmethod
    def __lt__(self) -> bool:
        pass

def BinarySearchTreeNode:
    value: Comparable
    parent: BinarySearchTreeNode
    left: BinarySearchTreeNode
    right: BinarySearchTreeNode

    def validate(self) -> None:
        if self.right.value < self.left.value:
            raise Exception()
```

Values are removed from a binary search tree by replacing them with its inorder successor (see Key Algorithms).

An *AVL tree* is a binary search tree which maintains a fourth invariant:

4. The AVL tree is always balanced.

This ensures that the height of the tree is kept low so that binary search takes O(log n) time, at the expense of raising the worst-case insertion runtime to O(n).

A *red-black* tree is another self-balancing binary search tree which follows its own set of rules in addition to the BST invariants:

1. Every node has a single-bit flag, either `false` ("red") or `true` ("black").
1. The root of the tree is always "black".
1. A "red" node cannot have a red parent or red child.
1. Every path from a node to any of its descendants' `null` children has the same number of black "black" nodes.
1. All `null` children are "black".

There are many other tree types, as well as more information about trees described here (such as insert/remove/serach algorithms), which you can learn more about [here](https://www.geeksforgeeks.org/types-of-trees-in-data-structures/).

## Key Algorithms

### Traversals

There are four types of tree traversals:

1. Inorder Traversal: Traverse the left subtree, then the current, node, then traverse the right subtree
1. Preorder Traversal: Visit the current node, then traverse the left subtree, then traverse the right subtree
1. Postorder Traversal: Traverse the left subtree, then traverse the right subtree, then visit the current node
1. Level-order Traversal: Using a queue, visit a node and enqueue its children to be visited

```python
from some_source import visit

def inorder(root: BinaryTreeNode) -> None:
    if root is None:
        return
    inorder(root.left)
    visit(root)
    inorder(root.right)

def preorder(root: BinaryTreeNode) -> None:
    if root is None:
        return
    visit(root)
    preorder(root.left)
    preorder(root.right)

def postorder(root: BinaryTreeNode) -> None:
    if root is None:
        return
    postorder(root.left)
    postorder(root.right)
    visit(root)

from collections import deque
def levelorder(root: BinaryTreeNode) -> None:
    queue: deque[BinaryTreeNode] = deque([root])

    while len(queue) > 0:
        node: BinaryTreeNode = queue.popleft()
        if node is None:
            continue
        visit(node)

        queue.append(node.left)
        queue.append(node.right)
```

### Binary Search Tree Implementation

```python
class BinarySearchTree:

    root: BinarySearchTreeNode
    size: int

    def __init__(self):
        self.root = None
        self.size = 0

    def insert(self, 
                value: Comparable, 
                curr: BinarySearchTreeNode = self.root,
                parent: BinarySearchTreeNode = None) -> BinarySearchTreeNode:
        """
        Inserts the new node, or returns the old one if it already exists
        """
        res: BinarySearchTreeNode = None
        # base case 1: no root
        if curr is None and parent is None:
            res = BinarySearchTreeNode()
            res.value = value
            self.root = res
            size += 1
        # base case 2: curr null but parent not
        elif curr is None and parent is not None:
            res = BinarySearchTreeNode()
            res.value = value
            if value < parent.value:
                res = parent.left
            else:
                res = parent.right
            size += 1
        # base case 3: curr not null and has matching value
        elif curr is not None and not (curr.value < value or value < curr.value):
            res = curr
        # recursive case: continue binary search
        else:
            if value < curr.value:
                res = self.insert(value, curr.left, curr)
            else:
                res = self.insert(value, curr.right, curr)
        return res
    
    def search(self, 
                value: Comparable, 
                curr: BinarySearchTreeNode = self.root) -> BinarySearchTreeNode | None:
        """
        Returns the node, or None if it is not found
        """
        res: BinarySearchTreeNode = None
        # base case: curr null or has matching value
        if curr is None or not (curr.value < value or value < curr.value):
            res = curr
        # recursive case: non-matching value
        else:
            res = self.search(value, curr.left) if value < curr.value else self.search(value, curr.right)
        return res

    def _delete_helper(self, 
                        root: BinarySearchTreeNode = self.root,
                        value: Comparable) -> BinarySearchTreeNode:
        if root is None:
            return root
        
        if value < root.value:
            root.left = self._delete_helper(root.left, value)
        elif root.value < value:
            root.right = self._delete_helper(root.right, value)
        else:
            if root.left is None:
                return root.right
            if root.right is None:
                return root.left
            
            def get_successor(node: BinarySearchTreeNode) -> BinarySearchTreeNode:
                """
                Find next greatest element in tree
                """
                node = node.right
                while node is not None and node.left is not None:
                    node = nod
            
            successor: BinarySearchTreeNode = get_successor(root)
            root.key = successor.key
            root.right = del_node(root.right, successor.key)
        
        if root == self.root:
            self.size -= 1

        return root

    def delete(self, value: Comparable) -> None:
        """
        Deletes the node if in list
        """
        self._delete_helper(value)

    def __len__(self):
        return self.size
```

## Problems

* [Invert Binary Tree](../code/226-InvertBinaryTree.md)
* [Maximum Depth of Binary Tree](../code/104-MaximumDepthBinaryTree.md)
* [Lowest Common Ancestor of a Binary Search Tree](../code/235-LowestCommonAncestorBinarySearchTree.md)
* [Kth Smallest Element in a BST](../code/230-KthSmallestElementBST.md)
* [Binary Tree Maximum Path Sum](../code/124-BinaryTreeMaximumPathSum.md)