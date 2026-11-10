---
title: Group Anagrams
number: 49
language: python
pattern: hashing
difficulty: medium
tags: [hashing, strings, sorting, dictionary]
---

# 49 Group Anagrams (Python)

Other language: [[49 Group Anagrams - CSharp]] · Pattern: [[Hashing]]

## What it asks

You get a list of words. Group together the words that are **anagrams**: words made of exactly the same letters in a different order, like `eat`, `tea` and `ate`. Return the groups in any order.

## Pattern and trigger

**Hashing** with a computed key.

**Trigger:** "group things that are the same in some way." Turn each word into a **key** that is identical for every member of its group, then use a dictionary from key to group.

For anagrams, sorting the letters gives that key: `eat`, `tea` and `ate` all sort to `aet`.

## Picture

```
eat → aet ─┐
tea → aet ─┼─ group "aet": [eat, tea, ate]
ate → aet ─┘
tan → ant ─┬─ group "ant": [tan, nat]
nat → ant ─┘
bat → abt ─── group "abt": [bat]
```

## Questions before coding

- Lowercase letters only? (Yes.)
- Can the list contain an empty string? (Yes; it sorts to `""` and forms its own group.)
- Does the order of groups or words matter? (No.)

## Trace table

`strs = ["eat", "tea", "tan", "ate", "nat", "bat"]`

| word | key = sorted letters | groups after this word |
|---|---|---|
| eat | aet | aet: [eat] |
| tea | aet | aet: [eat, tea] |
| tan | ant | aet: [eat, tea] · ant: [tan] |
| ate | aet | aet: [eat, tea, ate] · ant: [tan] |
| nat | ant | aet: [...] · ant: [tan, nat] |
| bat | abt | aet: [...] · ant: [...] · abt: [bat] |

## Solution

```python
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: list[str]) -> list[list[str]]:
        groups = defaultdict(list)   # a missing key starts as an empty list

        for word in strs:
            key = "".join(sorted(word))   # sorted() gives a list of letters; join glues them
            groups[key].append(word)

        return list(groups.values())
```

Two terms in that code:
- `defaultdict(list)` creates the missing key with an empty list the first time you touch it. It doesn't "allow empty keys"; it fills in a default **value**.
- In `"".join(...)`, the `""` is the separator placed between pieces. Here that's nothing.

## Complexity

- **Time:** O(n · k log k), for n words of length up to k. Each word is sorted.
- **Space:** O(n · k). Every letter of every word is stored once in the groups.

**Faster key:** count the letters instead of sorting. A tuple of 26 counts is the key, which makes the time O(n · k).

## Watch-outs

- Using the sorted **list** as a key: lists can't be dictionary keys in Python. Join it to a string or convert it to a tuple.
- Sorting hides a loop. Say "k log k per word" when giving the complexity.
