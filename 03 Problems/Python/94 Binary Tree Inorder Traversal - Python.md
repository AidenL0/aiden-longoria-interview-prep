---
title: Binary Tree Inorder Traversal
number: 94
language: python
pattern: trees
difficulty: easy
tags: [trees, stack, traversal]
---

# 94 Binary Tree Inorder Traversal (Python)

Pattern: [[Trees]]

## What it asks

You get the **root** (the top node) of a binary tree. Return the values in **inorder**: everything in the left subtree, then the node itself, then everything in the right subtree.

A **binary tree** is a set of nodes where each node has a value and up to two children, `left` and `right`. LeetCode gives each node as a `TreeNode` with `.val`, `.left` and `.right`. A missing child is `None`.

## Pattern and trigger

**Tree traversal**, depth first (go as deep as possible down one side before backing up).

**Trigger:** "visit every node in a set order." Preorder, inorder and postorder differ only in **when** you record the node: before, between, or after its children.

## Picture

```
        4
       / \
      2   6
     / \   \
    1   3   7
```

Inorder: 1, 2, 3, 4, 6, 7. On a **binary search tree** (left smaller, right bigger), inorder gives the values sorted.

## Questions before coding

- Can the root be `None`? (Yes: return `[]`.)
- Recursive or iterative? Both are O(n). The iterative version uses your own stack instead of the call stack.

## Trace table

The iterative version on the tree above. "Go left" pushes nodes until there is no left child.

| Step | current | Action | stack (bottom → top) | result |
|---|---|---|---|---|
| 1 | 4 | go left, pushing 4, 2, 1 | 4 2 1 | [] |
| 2 | None | pop 1, record it, go right (None) | 4 2 | [1] |
| 3 | None | pop 2, record it, go right to 3 | 4 | [1, 2] |
| 4 | 3 | go left, pushing 3 | 4 3 | [1, 2] |
| 5 | None | pop 3, record it, go right (None) | 4 | [1, 2, 3] |
| 6 | None | pop 4, record it, go right to 6 | empty | [1, 2, 3, 4] |
| 7 | 6 | push 6; pop 6, record it, go right to 7 | empty | [1, 2, 3, 4, 6] |
| 8 | 7 | push 7; pop 7, record it, go right (None) | empty | [1, 2, 3, 4, 6, 7] |

The loop stops when `current` is `None` and the stack is empty.

## Solution

```python
class Solution:
    def inorderTraversal(self, root: TreeNode | None) -> list[int]:
        result = []
        stack = []        # nodes we still have to come back to
        current = root    # a second name for the same node, not a copy

        while current is not None or stack:
            while current is not None:     # walk as far left as possible
                stack.append(current)
                current = current.left

            current = stack.pop()          # pop gives back the node object
            result.append(current.val)     # .val is its number
            current = current.right        # then handle the right side

        return result
```

**Recursive version** (shorter; same order):

```python
def inorder(node, result):
    if node is None:
        return
    inorder(node.left, result)
    result.append(node.val)      # move this line up for preorder, down for postorder
    inorder(node.right, result)
```

## Complexity

- **Time:** O(n). Each node is pushed and popped once.
- **Space:** O(h), where h is the tree's height. That's O(log n) for a balanced tree and O(n) for a tree shaped like a line.

## Watch-outs

- `current = root` doesn't copy the tree. Both names point to the same node.
- `stack.pop()` returns the node. You still need `.val` to get its number.
- Practice: write preorder (144) and postorder (145) by moving the one `append` line in the recursive version.
