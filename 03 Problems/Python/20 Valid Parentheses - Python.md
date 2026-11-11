---
title: Valid Parentheses
number: 20
language: python
pattern: stack
difficulty: easy
tags: [stack, strings]
status: solved
---

# 20 Valid Parentheses (Python)

Other language: [[20 Valid Parentheses - CSharp]] · Pattern: [[Stack]]

> **Solved.** I solved this one myself. My version pushed each opening bracket and looked up its partner in the dictionary; the version below pushes the closer it expects instead, so the match is one comparison.

## What it asks

You get a string made only of the brackets `( ) [ ] { }`. Return `True` if every opening bracket is closed by the same kind of bracket, in the right order. `"([])"` is valid; `"(]"` and `"(("` are not.

## Pattern and trigger

**Stack**: a pile where you only add to the top and take from the top (last in, first out).

**Trigger:** "the most recent unfinished thing must be finished first." The last bracket opened is the first one that must close.

## Picture

```
read:   (     [     ]     )
stack:  )     ) ]   )     (empty)
             push  pop   pop
```

This version pushes the **closer we expect** for each opener. When a closer arrives, it must match the top of the stack.

## Questions before coding

- Only bracket characters? (Yes.)
- Is an empty string valid? (Yes: nothing is left open.)
- What if a closer arrives when nothing is open? (Invalid right away.)

## Trace table

`s = "([]{})"`

| ch | Kind | Top of stack before | Action | Stack after |
|---|---|---|---|---|
| `(` | opener | — | push `)` | `)` |
| `[` | opener | `)` | push `]` | `) ]` |
| `]` | closer | `]` | match, pop | `)` |
| `{` | opener | `)` | push `}` | `) }` |
| `}` | closer | `}` | match, pop | `)` |
| `)` | closer | `)` | match, pop | empty |

The stack ends empty, so the answer is `True`.

## Solution

```python
class Solution:
    def isValid(self, s: str) -> bool:
        expected_closer = {"(": ")", "[": "]", "{": "}"}
        stack = []  # closers we are waiting for, newest on top

        for ch in s:
            if ch in expected_closer:
                stack.append(expected_closer[ch])   # remember what must close it
            elif stack and stack[-1] == ch:         # empty check comes first
                stack.pop()                          # matched the newest opener
            else:
                return False   # wrong closer, or nothing open to close

        return len(stack) == 0   # leftover openers mean invalid
```

Most textbooks map closers to openers instead. Mapping openers to their expected closers works just as well and makes the match check one comparison. Both are O(n).

## Complexity

- **Time:** O(n). Each character is pushed or popped at most once.
- **Space:** O(n). A string of only openers fills the stack.

## Where I got stuck

- The idea was mine: a stack, plus a dictionary pairing each opening bracket with its closing partner. I needed help with Python syntax and some implementation details.
- An AI reviewer first objected to keying the dictionary by the opening bracket, until I explained why it works.

## Watch-outs

- Order inside `stack and stack[-1] == ch` matters. Python stops at the first `False`, so the empty check protects `stack[-1]` from an `IndexError`.
- Two different failures: a closer with nothing open (inside the loop), and openers left over (after the loop). Check both.
