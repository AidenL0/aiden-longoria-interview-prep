---
title: Trees
tags: [pattern, trees, dfs, bfs]
---

# Trees

Back to [[Start Here]]

## What it is

A **binary tree** is made of nodes with a value and up to two children, `left` and `right`. The top node is the **root**; nodes with no children are **leaves**. LeetCode gives you the root as a `TreeNode` with `.val`, `.left` and `.right`.

Two ways to visit every node:
- **Depth-first search (DFS):** go as deep as possible down one branch before backing up. Uses recursion or a stack.
- **Breadth-first search (BFS):** visit the tree level by level. Uses a queue (`collections.deque` in Python, `Queue<T>` in C#).

## Why it matters

Trees show up in every interview loop, and almost all tree problems are one of a few shapes.

## Triggers and shapes

| The problem asks for | Shape |
|---|---|
| depth, height, "is it balanced," a sum from below | DFS that **returns a value up** from the children |
| comparing a node with its ancestors (a valid search tree, a path from the root) | DFS that **carries state down** as a parameter |
| diameter or best path through a node | DFS that returns one value while tracking another best answer |
| anything "per level" or "per row" | BFS with a queue |

## Templates

**DFS, returning a value up:**

```python
def height(node):
    if node is None:          # base case: an empty tree has height 0
        return 0
    return 1 + max(height(node.left), height(node.right))
```

**Traversal order** depends on where you record the node: before the children (preorder), between them (inorder), or after them (postorder).

**BFS by level:**

```python
from collections import deque
queue = deque([root])
while queue:
    for _ in range(len(queue)):   # everything in the queue now is one level
        node = queue.popleft()
        if node.left: queue.append(node.left)
        if node.right: queue.append(node.right)
```

## Problems

- [[94 Binary Tree Inorder Traversal - Python]]

## Watch-outs

- Every recursive function needs a base case (`if node is None`).
- Recursion uses O(h) space for the call stack, where h is the height. On a tree shaped like a line, that's O(n).
- A time-limit failure is rarely caused by recursion itself. Look for a hidden O(n) step inside it, like calling a separate height function from every node, or slicing a list.
