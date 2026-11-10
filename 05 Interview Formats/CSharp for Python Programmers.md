---
title: CSharp for Python Programmers
tags: [interview-formats, csharp, python, syntax]
---

# CSharp for Python Programmers

Back to [[Start Here]] · Roadmap: [[CSharp Roadmap]]

## What it is

A translation guide. C# is Python with the types written out, braces instead of indentation, and a semicolon after every statement. Learn the shape first; most of the rest is lookup.

## Why it matters

Some employers test in C# (common at companies using Microsoft's .NET). If you already think in Python, you don't need new algorithms, just reliable translation.

## The shape of every solution

```csharp
using System;
using System.Collections.Generic;   // List, Dictionary, HashSet, Queue, Stack
using System.Linq;                  // optional helpers like .Sum(), .OrderBy()
using System.Text;                  // StringBuilder

public class Solution
{
    public int[] TwoSum(int[] nums, int target)   // return type, name, typed parameters
    {
        // body
        return new int[] { -1, -1 };
    }
}
```

LeetCode adds the `using` lines for you; typing them again only gives a warning (CS0105). Other editors may not add them.

## Syntax side by side

| Concept | Python | C# |
|---|---|---|
| Variable | `x = 5` | `int x = 5;` or `var x = 5;` |
| Char versus string | both `"a"` | `'a'` is a `char`, `"a"` is a `string` |
| Nothing | `None` | `null` |
| Booleans | `True and not False` | `true && !false` |
| If | `if x > 0:` / `elif` | `if (x > 0) { } else if (...) { } else { }` |
| Counting loop | `for i in range(n):` | `for (int i = 0; i < n; i++) { }` |
| Backward loop | `for i in range(n-1, -1, -1):` | `for (int i = n - 1; i >= 0; i--) { }` |
| For-each | `for x in arr:` | `foreach (var x in arr) { }` |
| Length | `len(arr)` | `arr.Length` (arrays, strings), `list.Count` (collections) |
| Whole-number division | `7 // 2` | `7 / 2` (two ints give an int) |
| Infinity sentinel | `float('inf')` | `int.MaxValue` / `int.MinValue` |
| Max, absolute value | `max(a, b)`, `abs(x)` | `Math.Max(a, b)`, `Math.Abs(x)` |
| Slice | `s[1:4]` | `s.Substring(1, 3)` (start, **length**) |
| Join, split | `",".join(parts)`, `s.split(" ")` | `string.Join(",", parts)`, `s.Split(' ')` |
| Character checks | `c.isalnum()`, `s.lower()` | `char.IsLetterOrDigit(c)`, `s.ToLower()` |
| f-string | `f"{x} items"` | `$"{x} items"` |
| Lambda | `lambda x: x * 2` | `x => x * 2` |

## Collections and what they cost

| Python | C# | Key operations |
|---|---|---|
| `list` (fixed size) | `int[]`, `new int[n]` | `arr[i]` O(1); can't grow |
| `list` | `List<int>` | `Add` O(1), `list[i]` O(1), `Insert(0, x)` O(n), `Contains` O(n) |
| `dict` | `Dictionary<string, int>` | `d[k] = v`, `ContainsKey`, `TryGetValue`: O(1) |
| `set` | `HashSet<int>` | `Add` (returns `false` if present), `Contains`: O(1) |
| `deque` as a queue | `Queue<T>` | `Enqueue`, `Dequeue`, `Peek`: O(1) |
| `list` as a stack | `Stack<T>` | `Push`, `Pop`, `Peek`: O(1) |
| `heapq` | `PriorityQueue<TElement, TPriority>` | `Enqueue`, `Dequeue`: O(log n); a min-heap |
| building a `str` | `StringBuilder` | `Append` O(1), `ToString()` O(n) |

**Hidden loops in C#:** `list.Contains`, `string.Join`, `Substring`, `ToCharArray`, `new string(chars)`, `ToList()`, and LINQ calls like `.Sum()`, `.Max()`, `.OrderBy()` all touch every element. `.Count` (a property) is O(1); `.Count()` (a LINQ method) can be O(n).

## Gotchas that bite Python programmers

| Gotcha | What happens | Fix |
|---|---|---|
| `dict[key]` on a missing key | `KeyNotFoundException` | `ContainsKey`, `TryGetValue` or `GetValueOrDefault` |
| Changing a collection inside `foreach` over it | `InvalidOperationException` | Loop by index, or collect changes and apply them after |
| `s += c` in a loop | O(n²), same as Python | `StringBuilder` |
| `char` arithmetic | `'a' + 1` is the int 98 | `(char)('a' + 1)`; index letters with `c - 'a'` |
| `-7 / 2` and `-7 % 2` | `-3` and `-1` (Python gives `-4` and `1`) | C# rounds toward zero |
| `int` overflow | `int.MaxValue + 1` silently wraps to a negative | Use `long` for big sums |
| `var b = a;` with an array or list | A second name, not a copy | `(int[])a.Clone()` or `new List<int>(a)` |
| `==` on arrays or lists | Compares identity, not contents | `a.SequenceEqual(b)` (LINQ); `==` on strings is fine |
| `Substring(start, length)` | Second argument is a length, not an end | `s.Substring(i, j - i)` for Python's `s[i:j]` |
| Strings can't be changed | `s[0] = 'x'` won't compile | `ToCharArray()`, edit, then `new string(chars)` |

## A starter snag list

One line per surprise, from real practice. Keep your own and reread it before every session.

1. Scan compiler output for `error`, not `warning`.
2. `foreach` has no index. Use `for`, or a counter declared **before** the loop.
3. A `return` inside a loop without a condition ends the method on the first pass.
4. `return -1;` won't compile when the method returns `int[]` (CS0029). Use `new int[0]`.
5. `TryGetValue` on a missing key returns `false` and sets the value to 0, with no error. A wrong key fails silently.
6. `Dictionary.Add` throws on a duplicate key; `dict[key] = value` adds or overwrites.
7. Properties take no parentheses: `.Length`, `.Count`.
8. Objects need `new`: `new List<string>()`.
9. `TryGetValue(key, out int value)`: `out`, then the type, then the name.
10. `&&` and `||` stop early. Put the safety check on the left: `stack.Count > 0 && stack.Peek() == c`.
11. `Pop()` and `Peek()` on an empty stack throw at runtime.
12. `.Max()` and `.Sum()` need `using System.Linq;`. Writing the loop always works.
13. On a string, `Append` compiles (LINQ) and changes nothing. Assign: `s += c`.
14. LeetCode already defines `ListNode` and `TreeNode`; defining them again is CS0101.
15. "Any one is enough" is `||`; "all must hold" is `&&`.
16. Starting a max at `0` only works for non-negative numbers. Start at `nums[0]` or `int.MinValue`.

## Worked translation

[[49 Group Anagrams - Python]] next to [[49 Group Anagrams - CSharp]]: the algorithm doesn't change, only the ceremony. `sorted(word)` becomes three steps (to an array, sort it in place, back to a string), and `defaultdict` becomes a `ContainsKey` check.
