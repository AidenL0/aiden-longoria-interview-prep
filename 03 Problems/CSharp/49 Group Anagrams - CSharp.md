---
title: Group Anagrams
number: 49
language: csharp
pattern: hashing
difficulty: medium
tags: [hashing, strings, sorting, dictionary]
status: worked-example
---

# 49 Group Anagrams (C#)

Other language: [[49 Group Anagrams - Python]] · Pattern: [[Hashing]]

> **Worked example.** I solved this one in Python ([[49 Group Anagrams - Python]]); this C# version is a translation I haven't written myself yet. Try it yourself before reading on.

## What it asks

Group the words that are anagrams of each other (same letters, different order). `["eat", "tea", "tan", "ate", "nat", "bat"]` → `[["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]`, in any order.

## Pattern and trigger

**Hashing** with a computed key: sort each word's letters, and every anagram gets the same key. A `Dictionary<string, List<string>>` maps key → group.

**Trigger:** "group things that are the same in some way."

## Picture

```
"tea" → { 't','e','a' } → sort → { 'a','e','t' } → "aet"
```

In Python, `"".join(sorted(word))` is one line. In C# it's three steps: string to char array, sort it in place, back to a string.

## Questions before coding

- Lowercase only? (Yes.)
- Group or word order? (Doesn't matter.)

## Trace table

| word | chars after `Array.Sort` | key | groups |
|---|---|---|---|
| eat | a e t | "aet" | aet: [eat] |
| tea | a e t | "aet" | aet: [eat, tea] |
| tan | a n t | "ant" | aet: [eat, tea] · ant: [tan] |
| ate | a e t | "aet" | aet: [eat, tea, ate] · ant: [tan] |
| nat | a n t | "ant" | ... ant: [tan, nat] |
| bat | a b t | "abt" | ... abt: [bat] |

## Solution

```csharp
public class Solution {
    public IList<IList<string>> GroupAnagrams(string[] strs) {
        var groups = new Dictionary<string, List<string>>();

        foreach (string word in strs) {
            char[] chars = word.ToCharArray();   // strings can't be sorted in place
            Array.Sort(chars);                   // sorts the array itself
            string key = new string(chars);      // back to a string for the key

            if (!groups.ContainsKey(key)) {      // C# has no defaultdict: create it first
                groups[key] = new List<string>();
            }
            groups[key].Add(word);
        }

        var result = new List<IList<string>>();
        foreach (var group in groups.Values) {
            result.Add(group);
        }
        return result;
    }
}
```

`IList<IList<string>>` is the return type LeetCode declares. A `List` satisfies an `IList`, so returning a `List<IList<string>>` works.

## Complexity

- **Time:** O(n · k log k) for n words of length up to k (one sort per word).
- **Space:** O(n · k) to store every letter in the groups.

## Watch-outs

- `groups[key].Add(word)` on a key that doesn't exist yet throws `KeyNotFoundException`. Create the list first.
- `new string(chars)`, not `chars.ToString()`: the second gives the type name `System.Char[]`.
