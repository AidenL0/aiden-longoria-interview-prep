---
title: Valid Anagram
number: 242
language: csharp
pattern: hashing
difficulty: easy
tags: [hashing, counting, strings]
status: solved
---

# 242 Valid Anagram (C#)

Pattern: [[Hashing]] · Related: [[49 Group Anagrams - CSharp]]

> **Solved.** I solved this one myself.

## What it asks

Return `true` if string `t` uses exactly the same letters as string `s`, the same number of times each (an **anagram**). `"anagram"` and `"nagaram"` → `true`; `"rat"` and `"car"` → `false`.

## Pattern and trigger

**Frequency counting**: a tally of how many times each letter appears. With only the 26 lowercase letters, an `int[26]` works as the tally (slot 0 for `a`, slot 25 for `z`).

**Trigger:** "same letters, any order."

## Picture

```
count for s = "rat":  a:1  r:1  t:1
subtract t = "car":   a:0  r:0  t:1  c:−1   → not all zero → false
```

Add 1 for each letter of `s`, subtract 1 for each letter of `t`. Anagrams end with every slot at 0.

## Questions before coding

- Lowercase English letters only? (Yes, which is why `int[26]` works.)
- Different lengths? (Can't be anagrams: return `false` right away.)

## Trace table

`s = "aab"`, `t = "aba"`. Slot index is `c - 'a'`.

| i | s[i] | +1 slot | t[i] | −1 slot | counts (a, b) |
|---|---|---|---|---|---|
| 0 | a | 0 | a | 0 | (0, 0) |
| 1 | a | 0 | b | 1 | (1, −1) |
| 2 | b | 1 | a | 0 | (0, 0) |

All zero, so `true`.

## Solution

```csharp
public class Solution {
    public bool IsAnagram(string s, string t) {
        if (s.Length != t.Length) {
            return false;
        }

        int[] counts = new int[26];   // 26 zeros, one per letter

        for (int i = 0; i < s.Length; i++) {
            counts[s[i] - 'a']++;     // 'a' is character code 97, so 'a' - 'a' = 0
            counts[t[i] - 'a']--;
        }

        for (int j = 0; j < 26; j++) {
            if (counts[j] != 0) {     // j is which slot; counts[j] is what's in it
                return false;
            }
        }
        return true;
    }
}
```

## Complexity

- **Time:** O(n) for strings of length n.
- **Space:** O(1). The array is always 26 slots, however long the strings are.

**Unicode follow-up:** for any characters, use `Dictionary<char, int>` with `GetValueOrDefault(c) + 1` instead of the array.

## Where I got stuck

- I knew the approach but got stuck turning it into C#. I asked an AI two syntax questions: how to turn a string into an array of characters, and how to compare two arrays.
- Later I learned a string can be indexed directly (`s[i]` is a `char`), so no conversion is needed, and that it's `- 'a'`, not `- 0`, that turns a letter into a slot from 0 to 25.
- In a later rewrite I mixed up `j` (which slot) and `counts[j]` (what's in it).

## Watch-outs

- `new int[26]`: the type after `new` is `int`, not the variable's name.
- `s[i] - 'a'`, not `s[i] - 0`. `0` is a number; `'a'` is the character whose code is 97.
- Strings index directly: `s[i]` is a `char`. `ToCharArray()` is only needed for sorting.
