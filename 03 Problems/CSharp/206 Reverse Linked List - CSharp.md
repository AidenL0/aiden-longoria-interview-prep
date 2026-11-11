---
title: Reverse Linked List
number: 206
language: csharp
pattern: linked-lists
difficulty: easy
tags: [linked-lists, pointers]
status: solved
---

# 206 Reverse Linked List (C#)

Pattern: [[Linked Lists]]

> **Solved.** I solved this one myself.

## What it asks

You get the `head` (first node) of a **singly linked list**: a chain of nodes where each node holds a value (`val`) and a pointer to the next node (`next`). The last node's `next` is `null`. Reverse the chain and return the new head.

## Pattern and trigger

**Pointer reversal** with three variables: `previous`, `current` and `next`.

**Trigger:** "reverse" or "re-link" a linked list. You can't jump to a position like in an array, so you walk the chain once and flip each arrow as you pass it.

## Picture

```
before:  1 → 2 → 3 → null
after:   null ← 1 ← 2 ← 3      (3 is the new head)
```

## Questions before coding

- Empty list (`head == null`)? (Return `null`.)
- One node? (It's already reversed.)
- Make a new list or re-link the old one? (Re-link: O(1) extra space.)

## Trace table

`1 → 2 → 3`. Each row is one pass through the loop.

| Pass | current | next (saved) | flip | previous after | current after |
|---|---|---|---|---|---|
| 1 | 1 | 2 | 1.next = null | 1 | 2 |
| 2 | 2 | 3 | 2.next = 1 | 2 | 3 |
| 3 | 3 | null | 3.next = 2 | 3 | null |

`current` is `null`, so the loop ends. Return `previous`, which is node 3.

## Solution

```csharp
/**
 * LeetCode defines ListNode for you; don't define it again:
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 * }
 */
public class Solution {
    public ListNode ReverseList(ListNode head) {
        ListNode previous = null;
        ListNode current = head;

        while (current != null) {             // stop after the last node
            ListNode next = current.next;     // 1. save the rest of the chain
            current.next = previous;          // 2. flip this node's arrow
            previous = current;               // 3. step previous forward first...
            current = next;                   // 4. ...then current
        }

        return previous;   // the old tail is the new head
    }
}
```

## Complexity

- **Time:** O(n), one pass.
- **Space:** O(1), three pointer variables.

## Where I got stuck

- I needed explanations for two things: the loop condition, and an update I'd left out.
- Order matters in the pointer updates: `previous = current;` has to come before `current = next;`, or you lose your place in the list.
- LeetCode's `ListNode` class sits in a comment above the solution. Uncommenting it is a compile error, because the class would be defined twice.

## Watch-outs

- Save `current.next` **before** flipping it, or the rest of the chain is lost.
- The order of steps 3 and 4 matters: `previous = current` must come before `current = next`.
- Loop on `current != null`, not `current.next != null`, or the last node never gets flipped.
- Every `.` asks "is the thing on my left not `null`?" `head.next.next` on a one-node list throws `NullReferenceException`.
- Uncommenting LeetCode's `ListNode` block defines the class twice (error CS0101).
