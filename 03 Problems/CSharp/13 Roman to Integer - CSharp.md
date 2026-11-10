---
title: Roman to Integer
number: 13
language: csharp
pattern: hashing
difficulty: easy
tags: [strings, lookup-table, dictionary]
---

# 13 Roman to Integer (C#)

Pattern: [[Hashing]]

## What it asks

Convert a Roman numeral string, like `"MCMXCIV"`, into an integer (1994). The symbols are I = 1, V = 5, X = 10, L = 50, C = 100, D = 500, M = 1000. Normally you add them left to right. But when a smaller symbol comes **before** a bigger one, it's subtracted: `IV` is 4, `CM` is 900.

## Pattern and trigger

**Lookup table** (a dictionary from symbol to value) plus a **look-ahead**: compare each symbol with the one after it.

**Trigger:** fixed symbol values, with a rule that depends on the neighbor.

## Picture

```
M   C   M   X   C   I   V
+1000 −100 +1000 −10 +100 −1 +5   = 1994
```

A symbol is subtracted exactly when the next symbol is bigger.

## Questions before coding

- Is the input always a valid numeral? (Yes.)
- What happens at the last symbol? (There's no next symbol, so it's always added. Guard the look-ahead so you don't read past the end.)

## Trace table

`s = "MCMXCIV"`

| i | s[i] | value | next value | Rule | total |
|---|---|---|---|---|---|
| 0 | M | 1000 | 100 | add | 1000 |
| 1 | C | 100 | 1000 | subtract | 900 |
| 2 | M | 1000 | 10 | add | 1900 |
| 3 | X | 10 | 100 | subtract | 1890 |
| 4 | C | 100 | 1 | add | 1990 |
| 5 | I | 1 | 5 | subtract | 1989 |
| 6 | V | 5 | none | add | 1994 |

## Solution

```csharp
public class Solution {
    public int RomanToInt(string s) {
        var value = new Dictionary<char, int> {
            { 'I', 1 }, { 'V', 5 }, { 'X', 10 }, { 'L', 50 },
            { 'C', 100 }, { 'D', 500 }, { 'M', 1000 }
        };

        int total = 0;
        for (int i = 0; i < s.Length; i++) {
            int current = value[s[i]];

            // look ahead only if there is a next symbol (the left side guards the right)
            if (i + 1 < s.Length && current < value[s[i + 1]]) {
                total -= current;
            } else {
                total += current;
            }
        }
        return total;
    }
}
```

`'I'` in single quotes is a `char` (one character); `"I"` is a `string`. The dictionary is keyed by `char` because `s[i]` gives a `char`.

## Complexity

- **Time:** O(n), one pass.
- **Space:** O(1). The table always has seven entries.

## Watch-outs

- The loop header decides which elements you visit. Keep it `i < s.Length` and guard the look-ahead inside the loop; don't shrink the loop to `s.Length - 1`, or you'll skip the last symbol.
- `&&` checks left to right and stops early. Put the bounds check `i + 1 < s.Length` on the left so `s[i + 1]` never runs off the end.
