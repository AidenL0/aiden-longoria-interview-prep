---
title: Linked Lists
tags: [pattern, linked-lists, pointers]
---

# Linked Lists

Back to [[Start Here]]

## What it is

A **linked list** is a chain of nodes. Each node holds a value (`val`) and a pointer to the next node (`next`); the last node points to nothing (`None` in Python, `null` in C#). You only hold the first node, the **head**.

Unlike an array, there's no jumping to position `i`: you follow `next` one node at a time, so reaching position `i` is O(i).

## Why it matters

Linked-list problems test whether you can update pointers in the right order without losing part of the chain. Reversing one is a classic whiteboard question.

## Triggers

- The input is a `ListNode` head.
- "Reverse," "merge," "remove the nth node," "find the middle," "detect a cycle."

## Templates

**Walk the list:**

```python
current = head
while current is not None:
    # use current.val
    current = current.next
```

**Reverse it:** save the next node, flip the pointer, step forward. See [[206 Reverse Linked List - CSharp]].

**Slow and fast pointers:** `slow` moves one step and `fast` moves two. When `fast` reaches the end, `slow` is in the middle; if they ever meet, there's a cycle.

## Problems

- [[206 Reverse Linked List - CSharp]]

## Watch-outs

- Every `.next` asks "is the thing on my left not null?" Guard in order: `head is not None and head.next is not None`.
- Save `current.next` **before** you overwrite it.
- Draw the boxes and arrows. Pointer bugs are much easier to see on paper.
