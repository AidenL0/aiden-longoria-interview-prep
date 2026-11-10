---
title: Valid Parentheses
number: 20
language: csharp
pattern: stack
difficulty: easy
tags: [stack, strings]
---

# 20 Valid Parentheses (C#)

Other language: [[20 Valid Parentheses - Python]] · Pattern: [[Stack]]

## What it asks

A string holds only `( ) [ ] { }`. Return `true` if every opener is closed by the same kind, in the right order.

## Pattern and trigger

**Stack** (`Stack<char>` in C#: `Push` adds to the top, `Pop` removes the top, `Peek` reads it).

**Trigger:** the newest unfinished opener must close first.

## Picture

```
read:   {     (     )     }
stack:  }     } )   }     (empty)
```

Push the closer you expect for each opener; a closer must match the top.

## Questions before coding

- Empty string? (Valid.)
- A closer when the stack is empty? (Invalid. Check `Count > 0` before `Peek` or `Pop`.)

## Trace table

`s = "{()}"`

| ch | Count before | Action | Stack after (top on the right) |
|---|---|---|---|
| `{` | 0 | push `}` | `}` |
| `(` | 1 | push `)` | `} )` |
| `)` | 2 | top is `)`, pop | `}` |
| `}` | 1 | top is `}`, pop | empty |

`Count == 0` at the end, so `true`.

## Solution

```csharp
public class Solution {
    public bool IsValid(string s) {
        var stack = new Stack<char>();   // closers we're waiting for

        foreach (char ch in s) {
            if (ch == '(') stack.Push(')');
            else if (ch == '[') stack.Push(']');
            else if (ch == '{') stack.Push('}');
            // ch is a closer: it must match the newest expected closer
            else if (stack.Count > 0 && stack.Peek() == ch) stack.Pop();
            else return false;
        }

        return stack.Count == 0;   // leftover openers mean invalid
    }
}
```

## Complexity

- **Time:** O(n). Each character is pushed or popped at most once.
- **Space:** O(n) in the worst case (all openers).

## Watch-outs

- `Pop()` and `Peek()` on an empty stack throw `InvalidOperationException` at runtime. The compiler won't warn you.
- `stack.Count > 0 && stack.Peek() == ch`: the safety check goes on the left, because `&&` stops at the first `false`.
- `Count` is a property: no parentheses.
