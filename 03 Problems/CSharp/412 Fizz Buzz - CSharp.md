---
title: Fizz Buzz
number: 412
language: csharp
pattern: simulation-and-math
difficulty: easy
tags: [simulation, modulo, strings]
status: solved
---

# 412 Fizz Buzz (C#)

Pattern: [[Simulation and Math]]

> **Solved.** I solved this one myself.

## What it asks

For every number from 1 to `n`, add a string to a list:
- `"FizzBuzz"` if the number is divisible by both 3 and 5,
- `"Fizz"` if divisible by 3,
- `"Buzz"` if divisible by 5,
- otherwise the number itself, as text.

## Pattern and trigger

**Simulation**: do exactly what the rules say, in order. The tool is the **modulo operator** `%`, which gives the remainder of a division. `x % 3 == 0` means "x divides evenly by 3."

**Trigger:** a list of rules applied to each number in turn.

## Picture

```
1  2  Fizz  4  Buzz  Fizz  7  8  Fizz  Buzz  11  Fizz  13  14  FizzBuzz
```

## Questions before coding

- Start at 0 or 1? (1, and include `n`.)
- Which rule wins when two apply? (Check "both" first, or 15 would print `Fizz`.)

## Trace table

| i | i % 3 | i % 5 | Rule that fires | Added |
|---|---|---|---|---|
| 3 | 0 | 3 | divisible by 3 | Fizz |
| 5 | 2 | 0 | divisible by 5 | Buzz |
| 7 | 1 | 2 | none | "7" |
| 15 | 0 | 0 | both | FizzBuzz |

## Solution

```csharp
public class Solution {
    public IList<string> FizzBuzz(int n) {
        var answer = new List<string>();

        for (int i = 1; i <= n; i++) {           // 1 through n, inclusive
            if (i % 3 == 0 && i % 5 == 0) {      // most specific rule first
                answer.Add("FizzBuzz");
            } else if (i % 3 == 0) {
                answer.Add("Fizz");
            } else if (i % 5 == 0) {
                answer.Add("Buzz");
            } else {
                answer.Add(i.ToString());        // the number as text
            }
        }

        return answer;
    }
}
```

## Complexity

- **Time:** O(n).
- **Space:** O(n) for the output list (output space usually isn't counted as "extra").

## Where I got stuck

- My first version built a list of the numbers 1 to n just to loop over it. The loop variable already counts for you, so the version below skips the extra list.

## Watch-outs

- Rule order: "both" must come before the single checks.
- Don't build a list of 1 to n just to loop over it. The loop variable already is the sequence.
- On LeetCode, don't type `using System;` again: it's already there, and adding it gives warning CS0105. Scan the output for the word `error`, not `warning`.
