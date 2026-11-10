---
title: Longest Common Prefix
number: 14
language: csharp
pattern: arrays-and-indexing
difficulty: easy
tags: [strings, vertical-scan, nested-loops]
---

# 14 Longest Common Prefix (C#)

Pattern: [[Arrays and Indexing]]

## What it asks

You get an array of words. Return the longest **prefix** (starting piece) they all share. `["flower", "flow", "flight"]` → `"fl"`. If they share nothing, return `""`.

## Pattern and trigger

**Vertical scan**: line the words up and read down one column (letter position) at a time. Stop at the first column where a word is too short or a letter differs.

**Trigger:** "common to all," compared position by position.

## Picture

```
column:  0  1  2
flower   f  l  o
flow     f  l  o
flight   f  l  i   ← column 2 differs, stop
prefix:  f  l
```

## Questions before coding

- Can the array be empty? (LeetCode guarantees at least one word.)
- Can a word be empty? (Yes, then the answer is `""`.)
- Words of different lengths? (Yes. A word running out ends the prefix.)

## Trace table

`strs = { "flower", "flow", "flight" }`. Column `i`, word `j`.

| i | c = strs[0][i] | j = 1 | j = 2 | prefix after column |
|---|---|---|---|---|
| 0 | f | f ✓ | f ✓ | "f" |
| 1 | l | l ✓ | l ✓ | "fl" |
| 2 | o | o ✓ | **i ✗** | return "fl" |

## Solution

```csharp
public class Solution {
    public string LongestCommonPrefix(string[] strs) {
        string prefix = "";

        // i = which letter position (column); strs[0] sets the most columns possible
        for (int i = 0; i < strs[0].Length; i++) {
            char c = strs[0][i];

            // j = which word (row), starting with the second word
            for (int j = 1; j < strs.Length; j++) {
                // a word that ran out, or a different letter, ends the prefix
                if (i >= strs[j].Length || strs[j][i] != c) {
                    return prefix;
                }
            }

            prefix += c;   // once per column: after the inner loop, not inside it
        }

        return prefix;
    }
}
```

## Complexity

- **Time:** O(n · m) comparisons for n words and a prefix of length m. Each `prefix += c` also copies the string; for long prefixes, a `StringBuilder` or `strs[0].Substring(0, i)` at the end avoids that.
- **Space:** O(m) for the prefix.

## Watch-outs

All four happened while learning this one:

- Strings can't be changed in place. `prefix.Append(c)` compiles (it's a LINQ method that returns a new sequence) but changes nothing. Assign the result: `prefix += c`.
- Work that should happen once per column goes **after** the inner loop.
- Loop order isn't index order: `strs[j][i]` is always word first, then letter. Read each index out loud.
- `strs.Length` is the number of words; `strs[j].Length` is the length of word `j`.
