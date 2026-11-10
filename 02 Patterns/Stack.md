---
title: Stack
tags: [pattern, stack]
---

# Stack

Back to [[Start Here]]

## What it is

A **stack** is a pile: you add to the top and take from the top, so the last thing in is the first thing out. In Python, a list with `append` and `pop`. In C#, `Stack<T>` with `Push`, `Pop` and `Peek` (look at the top without removing it).

## Why it matters

Stacks track "unfinished business" in the order it must be finished: brackets, nested structures, undo history, and walking a tree without recursion.

## Triggers

- Matching pairs that nest: brackets, tags.
- "The most recent unfinished thing must be finished first."
- "Next greater element" style questions (a stack kept in sorted order).
- Turning a recursive solution into a loop.

## Template

```python
stack = []
for item in items:
    if opens_something(item):
        stack.append(item)
    elif stack and matches(stack[-1], item):
        stack.pop()
    else:
        return False
return len(stack) == 0
```

## Problems

- [[20 Valid Parentheses - Python]] · [[20 Valid Parentheses - CSharp]]
- [[94 Binary Tree Inorder Traversal - Python]]: a stack instead of recursion

## Watch-outs

- Popping or peeking an empty stack is an error (Python `IndexError`, C# `InvalidOperationException`). Check that it's non-empty first, on the **left** of `and` / `&&`.
- Check for leftovers after the loop: an empty stack at the end often matters as much as matches during it.
