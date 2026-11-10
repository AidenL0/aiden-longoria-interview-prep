---
title: Count Zero Request Servers
number: 2747
language: csharp
pattern: sliding-window
difficulty: medium
tags: [sliding-window, sorting, offline-queries, counting]
---

# 2747 Count Zero Request Servers (C#)

Pattern: [[Sliding Window]] · Builds on: [[2418 Sort the People - CSharp]], [[219 Contains Duplicate II - CSharp]], [[904 Fruit Into Baskets - CSharp]]

## What it asks

There are `n` servers, numbered 1 to `n`. `logs[i] = [serverId, time]` says that server got a request at that time. For each query time `q`, count the servers that got **no** requests during the time range `[q − x, q]` (both ends included).

## Pattern and trigger

Four ideas together, each practiced in an easier problem first:

1. **Sort the logs by time** so a window over them only ever moves right.
2. **Answer the queries in sorted order** but write each answer to its original slot (the "carry the original index" trick from 2418). Answering out of order like this is called handling queries **offline**.
3. A **sliding window** over the sorted logs: requests with time in `[q − x, q]`.
4. A **counting board** that knows how many *different* servers are in the window (from 904).

The answer for each query is `n − (servers active in the window)`.

**Trigger:** many range questions over the same timeline. Sorting both the events and the questions lets one window serve them all.

## Picture

`n = 3`, `logs = [[1,3], [2,6], [1,5]]`, `x = 5`, `queries = [10, 11]`

```
time:      3       5   6               10  11
logs:     s1      s1  s2
q = 10:            [5 ............... 10]   active {1, 2} → 3 − 2 = 1
q = 11:                [6 .............. 11] active {2}  → 3 − 1 = 2
```

## Questions before coding

- Are both ends of `[q − x, q]` included? (Yes.)
- Are the logs sorted? Are the queries? (Neither. Sort both, and remember where each query came from.)
- Can one server appear many times in the window? (Yes. Count it once: that's why the board counts distinct servers.)

## Trace table

Logs sorted by time: `(s1, 3), (s1, 5), (s2, 6)`. Queries sorted: 10 (from slot 0), 11 (from slot 1).

| Query | Add logs with time ≤ q | Remove logs with time < q − x | count | active | answer |
|---|---|---|---|---|---|
| 10 | (s1,3) (s1,5) (s2,6) | time < 5: remove (s1,3) | s1:1, s2:1 | 2 | answer[0] = 1 |
| 11 | none left | time < 6: remove (s1,5) | s1:0, s2:1 | 1 | answer[1] = 2 |

## Solution

```csharp
public class Solution {
    public int[] CountServers(int n, int[][] logs, int x, int[] queries) {
        // 1. sort logs by time (each log is [serverId, time])
        Array.Sort(logs, (a, b) => a[1].CompareTo(b[1]));

        // 2. sort query times, carrying each one's original index
        int[] sortedQueries = (int[])queries.Clone();   // a real copy, not an alias
        int[] order = new int[queries.Length];
        for (int i = 0; i < order.Length; i++) order[i] = i;
        Array.Sort(sortedQueries, order);

        int[] count = new int[n + 1];   // requests per server inside the window
        int active = 0;                 // servers with count > 0
        int left = 0, right = 0;        // the window is logs[left .. right - 1]
        int[] answer = new int[queries.Length];

        for (int k = 0; k < sortedQueries.Length; k++) {
            int q = sortedQueries[k];

            // grow: take in every log at or before q
            while (right < logs.Length && logs[right][1] <= q) {
                int server = logs[right][0];
                count[server]++;
                if (count[server] == 1) active++;   // 0 -> 1: a newly active server
                right++;
            }

            // shrink: drop every log before q - x
            while (left < right && logs[left][1] < q - x) {
                int server = logs[left][0];
                count[server]--;
                if (count[server] == 0) active--;   // 1 -> 0: no longer active
                left++;
            }

            answer[order[k]] = n - active;   // back to the query's original slot
        }
        return answer;
    }
}
```

`(a, b) => a[1].CompareTo(b[1])` is a **lambda**, a tiny unnamed method. It tells `Array.Sort` how to compare two logs: by their time.

## Complexity

- **Time:** O(L log L + Q log Q) for L logs and Q queries. The sorting dominates; both window edges only move forward, so the sliding is O(L + Q).
- **Space:** O(n + Q) for the counting board and the query arrays.

## Watch-outs

- `int[] sortedQueries = queries;` is an alias: sorting it scrambles `queries` too. Use `Clone()`.
- The active count changes only on a 0 → 1 or 1 → 0 transition. Every other add or remove leaves it alone.
- The range includes both ends: keep logs with `time >= q - x`, so drop those with `time < q - x`.
