---
title: Valid Palindrome
number: 125
language: csharp
pattern: two-pointers
difficulty: easy
tags: [two-pointers, strings]
status: solved
---

# 125 Valid Palindrome (C#)

Pattern: [[Two Pointers]]

> **Solved.** I solved this one myself. My version built a cleaned-up copy of the string first and compared that from both ends; the version below skips the extra characters in place, so it needs no copy.

## What it asks

A **palindrome** reads the same forward and backward. Ignore everything except letters and digits, and ignore upper versus lower case. Is the string a palindrome? `"A man, a plan, a canal: Panama"` → `true`; `"race a car"` → `false`.

## Pattern and trigger

**Converging two pointers**: one index starts at the left end, one at the right, and they walk toward each other comparing as they go.

**Trigger:** "compare the start with the end," with no extra copy needed.

## Picture

```
"A man, a plan, a canal: Panama"
 ^                             ^
 left                      right    'A' vs 'a' → equal, move both inward
```

Each pointer skips anything that isn't a letter or digit.

## Questions before coding

- What counts? (Letters and digits. Spaces and punctuation are skipped.)
- Case? (Ignored: compare lowercase versions.)
- A string with no letters at all, like `" "`? (`true`: nothing to disagree.)

## Trace table

`s = "ab, A"` (letters: a b a)

| left | s[left] | right | s[right] | Action |
|---|---|---|---|---|
| 0 | a | 4 | A | a = a, move both |
| 1 | b | 3 | (space) | skip right → 2 |
| 1 | b | 2 | , | skip right → 1 |
| 1 | b | 1 | b | same spot, b = b; move both, so left > right and the loop ends |

Result: `true`.

## Solution

```csharp
public class Solution {
    public bool IsPalindrome(string s) {
        int left = 0;
        int right = s.Length - 1;

        while (left < right) {
            // skip anything that isn't a letter or digit
            while (left < right && !char.IsLetterOrDigit(s[left])) left++;
            while (left < right && !char.IsLetterOrDigit(s[right])) right--;

            if (char.ToLower(s[left]) != char.ToLower(s[right])) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```

## Complexity

- **Time:** O(n). Each pointer moves across the string at most once.
- **Space:** O(1). Building a cleaned copy and reversing it also works, but costs O(n) space.

## Where I got stuck

- The idea was easy; the C# was not. I wrote `k.IsLetterOrDigit` first. It's `char.IsLetterOrDigit(k)`: some methods belong to the type (`char`), not to the value.
- I had to ask whether `foreach (char k in s)` gives the character or its position. It gives the character.
- Adding characters onto a string one at a time is what `StringBuilder` is for; I looked it up mid-problem.

## Watch-outs

- The skip loops need their own `left < right` check, or a string of only punctuation runs a pointer off the end.
- `char.IsLetterOrDigit` and `char.ToLower` are called on the `char` type, with the character passed in.
