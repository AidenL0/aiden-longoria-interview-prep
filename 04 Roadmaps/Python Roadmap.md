---
title: Python Roadmap
tags: [roadmap, python]
---

# Python Roadmap

Back to [[Start Here]] · Fundamentals: [[Big O Notation]], [[Complexity]], [[Python Comprehensions]] · Patterns: the `02 Patterns` folder · C# version: [[CSharp Roadmap]]

**What this is:** the main study track, in Python. Problems that already have a worked note in this vault are in the `03 Problems/Python` folder; work them alongside their phase.

A pattern-first progression from zero to interview-ready. ~130 problems, ordered so each one builds on the last.

**The core idea:** companies don't ask 3,000 different questions. They ask ~15 patterns wearing different costumes. Learn the patterns, and unseen problems become recognizable.

---

## How to use this list

**Per problem:**
1. Read it. Restate it out loud in your own words. If you can't, you don't understand it yet.
2. Think for **15 minutes max**. Say your approach out loud before typing — this is the actual interview skill.
3. Stuck after 15 min? Look at the solution. *Then close it and rewrite from scratch.* Reading a solution teaches nothing; reproducing it does.
4. Write down the **pattern name** and the **trigger** — the thing in the problem statement that should have tipped you off ("sorted array" → binary search or two pointers).
5. State the time and space complexity before you move on. Every time. Interviewers always ask.

**Spaced repetition:** redo any problem you needed help on after 3 days, then 10 days. Don't grind new problems while old ones are still shaky.

**Volume:** 2–3 problems/day beats 10 on Saturday. Consistency compounds here more than in almost any other skill.

---

## Phase 0 — Prerequisites (do this first, ~2 days)

Don't skip this. Most early frustration is Python fluency, not algorithms.

**Know cold:**
- [[Big O Notation]]: O(1), O(log n), O(n), O(n log n), O(n²), O(2ⁿ)
- Which Python operations cost what: `list.append` O(1) amortized, `list.insert(0,x)` O(n), `x in list` O(n), `x in set` O(1), `list.pop()` O(1) vs `list.pop(0)` O(n)
- Recursion + the call stack

**Python toolkit — memorize these imports:**

```python
from collections import defaultdict, Counter, deque
import heapq              # min-heap; use -x for max-heap
from functools import lru_cache, cache   # memoization in one line
import bisect             # bisect_left / bisect_right on sorted lists
import math

# Idioms that show up constantly
counts = Counter(s)                    # frequency map
graph = defaultdict(list)              # adjacency list, no KeyError
q = deque([root]); q.popleft()         # O(1) queue for BFS
heapq.heappush(h, (dist, node))        # tuples sort by first element
res = float('inf')                     # sentinel for minimums
for i, x in enumerate(arr): ...
for a, b in zip(arr, arr[1:]): ...     # adjacent pairs
arr.sort(key=lambda x: (x[0], -x[1]))  # multi-key sort
```

**Warm-up (not interview questions, just fluency):**
- 1929 Concatenation of Array
- 1920 Build Array from Permutation
- 27 Remove Element
- 1470 Shuffle the Array

---

## Phase 1 — Arrays, Hashing & Two Pointers

**The single highest-yield phase.** Hash maps solve an absurd fraction of interview problems.

### Tier 1A — Easy (start here)
| # | Problem | Pattern |
|---|---------|---------|
| 1 | Two Sum | Hash map complement — *the* canonical question |
| 217 | Contains Duplicate | Set for seen-before |
| 242 | Valid Anagram | Frequency counting |
| 268 | Missing Number | Math trick / XOR |
| 283 | Move Zeroes | Two pointers, in-place |
| 26 | Remove Duplicates from Sorted Array | Slow/fast pointer |
| 125 | Valid Palindrome | Converging pointers |
| 88 | Merge Sorted Array | Fill from the back |

### Tier 1B — Medium
| # | Problem | Pattern |
|---|---------|---------|
| 49 | Group Anagrams | Hash map with computed key |
| 347 | Top K Frequent Elements | Bucket sort / heap |
| 238 | Product of Array Except Self | Prefix & suffix products |
| 36 | Valid Sudoku | Multiple hash sets |
| 128 | Longest Consecutive Sequence | Set + only start from sequence heads |
| 167 | Two Sum II (sorted input) | Converging pointers |
| 15 | 3Sum | Sort + fixed element + two pointers |
| 11 | Container With Most Water | Greedy two pointers |
| 271 | Encode and Decode Strings | Length-prefix encoding *(premium)* |

**Checkpoint:** you should now reflexively think "hash map?" within 10 seconds of reading a problem.

---

## Phase 2 — Sliding Window, Stack & Binary Search

### Sliding Window
Trigger words: *"substring"*, *"subarray"*, *"contiguous"*, *"longest/shortest ... such that"*.

| # | Problem | Pattern |
|---|---------|---------|
| 121 | Best Time to Buy and Sell Stock | Track running min |
| 3 | Longest Substring Without Repeating Chars | Variable window + set |
| 424 | Longest Repeating Character Replacement | Window + max-frequency trick |
| 567 | Permutation in String | Fixed-size window + counts |
| 209 | Minimum Size Subarray Sum | Shrinking window |
| 76 | Minimum Window Substring | **Hard** — the boss fight of this pattern |
| 239 | Sliding Window Maximum | **Hard** — monotonic deque |

### Stack
Trigger words: *"matching pairs"*, *"next greater/smaller"*, *"nested"*, *"undo"*.

| # | Problem | Pattern |
|---|---------|---------|
| 20 | Valid Parentheses | Stack matching |
| 155 | Min Stack | Auxiliary stack — very common design Q |
| 150 | Evaluate Reverse Polish Notation | Stack evaluation |
| 22 | Generate Parentheses | Stack + backtracking preview |
| 739 | Daily Temperatures | **Monotonic stack** — learn this one properly |
| 853 | Car Fleet | Sort + monotonic stack |
| 84 | Largest Rectangle in Histogram | **Hard** — monotonic stack final form |

### Binary Search
Trigger words: *"sorted"*, *"minimize the maximum"*, *"find the smallest X such that..."*, or O(log n) required.

| # | Problem | Pattern |
|---|---------|---------|
| 704 | Binary Search | Nail the template first |
| 35 | Search Insert Position | `bisect_left` by hand |
| 74 | Search a 2D Matrix | Flatten indices |
| 875 | Koko Eating Bananas | **Binary search on the answer** — huge pattern |
| 153 | Find Minimum in Rotated Sorted Array | Modified binary search |
| 33 | Search in Rotated Sorted Array | Modified binary search |
| 981 | Time Based Key-Value Store | Binary search + design |
| 4 | Median of Two Sorted Arrays | **Hard** — skip on first pass |

**Binary search template — memorize it:**
```python
lo, hi = 0, len(arr) - 1
while lo <= hi:
    mid = (lo + hi) // 2
    if arr[mid] == target: return mid
    elif arr[mid] < target: lo = mid + 1
    else: hi = mid - 1
return -1
```

---

## Phase 3 — Linked Lists & Trees

### Linked Lists
Master three primitives and most problems fall out: **reverse**, **fast/slow pointers**, **dummy head node**.

| # | Problem | Pattern |
|---|---------|---------|
| 206 | Reverse Linked List | The primitive. Know iterative *and* recursive |
| 21 | Merge Two Sorted Lists | Dummy node |
| 141 | Linked List Cycle | Floyd's fast/slow |
| 876 | Middle of the Linked List | Fast/slow |
| 143 | Reorder List | Combines all three primitives |
| 19 | Remove Nth Node From End | Offset two pointers |
| 2 | Add Two Numbers | Carry handling |
| 138 | Copy List with Random Pointer | Hash map old→new |
| 287 | Find the Duplicate Number | Floyd's on an array (sneaky) |
| **146** | **LRU Cache** | **Hash map + doubly linked list — extremely frequent** |
| 23 | Merge k Sorted Lists | **Hard** — heap or divide & conquer |
| 25 | Reverse Nodes in k-Group | **Hard** |

### Trees
Almost every tree problem is DFS (recursion) or BFS (deque). Learn both templates, then it's variations.

| # | Problem | Pattern |
|---|---------|---------|
| 226 | Invert Binary Tree | DFS basics |
| 104 | Maximum Depth of Binary Tree | DFS returning a value |
| 100 | Same Tree | Parallel recursion |
| 572 | Subtree of Another Tree | Recursion within recursion |
| 543 | Diameter of Binary Tree | **Return one thing, track another** — key trick |
| 110 | Balanced Binary Tree | Same trick |
| 235 | Lowest Common Ancestor of a BST | Use BST ordering |
| 102 | Binary Tree Level Order Traversal | **BFS template** |
| 199 | Binary Tree Right Side View | BFS variation |
| 1448 | Count Good Nodes in Binary Tree | DFS carrying state down |
| 98 | Validate Binary Search Tree | Pass min/max bounds |
| 230 | Kth Smallest Element in a BST | In-order traversal |
| 105 | Construct Binary Tree from Preorder & Inorder | Divide & conquer |
| 124 | Binary Tree Maximum Path Sum | **Hard** — the return-vs-track trick again |
| 297 | Serialize and Deserialize Binary Tree | **Hard** — common at big tech |

**Two templates to memorize:**
```python
def dfs(node):
    if not node: return 0
    left, right = dfs(node.left), dfs(node.right)
    return 1 + max(left, right)

def bfs(root):
    q = deque([root])
    while q:
        for _ in range(len(q)):        # snapshot = one level
            node = q.popleft()
            if node.left:  q.append(node.left)
            if node.right: q.append(node.right)
```

---

## Phase 4 — Heaps, Backtracking, Tries & Graphs

### Heaps / Priority Queue
Trigger: *"top K"*, *"kth largest"*, *"median"*, *"schedule by priority"*.

| # | Problem | Pattern |
|---|---------|---------|
| 703 | Kth Largest Element in a Stream | Min-heap of size k |
| 1046 | Last Stone Weight | Max-heap via negation |
| 973 | K Closest Points to Origin | Heap with tuple keys |
| 215 | Kth Largest Element in an Array | Heap or quickselect |
| 621 | Task Scheduler | Greedy + heap |
| 355 | Design Twitter | Design + heap merge |
| 295 | Find Median from Data Stream | **Hard** — two heaps |

### Backtracking
One template, endless variations. Trigger: *"all combinations"*, *"all permutations"*, *"all possible"*.

| # | Problem | Pattern |
|---|---------|---------|
| 78 | Subsets | Include/exclude template |
| 46 | Permutations | Used-set tracking |
| 39 | Combination Sum | Reuse allowed |
| 40 | Combination Sum II | Skip duplicates |
| 90 | Subsets II | Skip duplicates |
| 17 | Letter Combinations of a Phone Number | Cartesian product |
| 79 | Word Search | Grid backtracking + undo |
| 131 | Palindrome Partitioning | Partitioning |
| 51 | N-Queens | **Hard** — the classic |

```python
def backtrack(start, path):
    res.append(path[:])          # copy! not path
    for i in range(start, len(nums)):
        path.append(nums[i])
        backtrack(i + 1, path)
        path.pop()               # undo — the "backtrack"
```

### Tries
| # | Problem | Pattern |
|---|---------|---------|
| 208 | Implement Trie (Prefix Tree) | Build it once, by hand |
| 211 | Design Add and Search Words | Trie + wildcard DFS |
| 212 | Word Search II | **Hard** — trie + grid backtracking |

### Graphs
Grids are graphs. `200 Number of Islands` may be the most-asked medium in existence.

| # | Problem | Pattern |
|---|---------|---------|
| 200 | Number of Islands | Grid DFS/BFS flood fill |
| 695 | Max Area of Island | Flood fill returning size |
| 133 | Clone Graph | DFS + hash map |
| 994 | Rotting Oranges | **Multi-source BFS** |
| 130 | Surrounded Regions | Reverse thinking — start from borders |
| 417 | Pacific Atlantic Water Flow | DFS from edges inward |
| 207 | Course Schedule | **Cycle detection / topological sort** |
| 210 | Course Schedule II | Topological sort (Kahn's algorithm) |
| 261 | Graph Valid Tree | Union-Find *(premium)* |
| 323 | Number of Connected Components | Union-Find *(premium)* |
| 684 | Redundant Connection | Union-Find |
| 127 | Word Ladder | **Hard** — BFS on implicit graph |

---

## Phase 5 — Dynamic Programming, Greedy & Intervals

DP is where most people stall. **The path through it:** every DP problem is recursion + memoization first. Write the brute-force recursion, add `@cache`, *then* convert to a table if asked. Don't start with tables.

### 1-D DP
| # | Problem | Pattern |
|---|---------|---------|
| 70 | Climbing Stairs | Fibonacci — the "hello world" of DP |
| 746 | Min Cost Climbing Stairs | Same shape, real cost |
| 198 | House Robber | Take-or-skip |
| 213 | House Robber II | Circular variant |
| 91 | Decode Ways | String DP |
| 322 | Coin Change | **Unbounded knapsack** — learn this cold |
| 139 | Word Break | DP over string positions |
| 152 | Maximum Product Subarray | Track min *and* max |
| 300 | Longest Increasing Subsequence | O(n²) then O(n log n) |
| 5 | Longest Palindromic Substring | Expand around center |
| 647 | Palindromic Substrings | Same |
| 416 | Partition Equal Subset Sum | **0/1 knapsack** |

### 2-D DP
| # | Problem | Pattern |
|---|---------|---------|
| 62 | Unique Paths | Grid DP |
| 1143 | Longest Common Subsequence | The 2-D template |
| 309 | Best Time to Buy/Sell with Cooldown | State machine DP |
| 518 | Coin Change II | Counting combinations |
| 494 | Target Sum | Knapsack variant |
| 72 | Edit Distance | Classic, asked often |
| 312 | Burst Balloons | **Hard** — interval DP |
| 10 | Regular Expression Matching | **Hard** |

### Greedy
| # | Problem | Pattern |
|---|---------|---------|
| 53 | Maximum Subarray | **Kadane's algorithm** — must know |
| 55 | Jump Game | Track furthest reach |
| 45 | Jump Game II | BFS-flavored greedy |
| 134 | Gas Station | Single-pass greedy |
| 846 | Hand of Straights | Counter + greedy |

### Intervals
Small pattern, disproportionately common. Almost always: **sort by start, then merge or count.**

| # | Problem | Pattern |
|---|---------|---------|
| 57 | Insert Interval | Three-phase scan |
| 56 | Merge Intervals | Sort + merge |
| 435 | Non-overlapping Intervals | Greedy by end time |
| 252 | Meeting Rooms | Overlap check *(premium)* |
| 253 | Meeting Rooms II | Heap / sweep line *(premium)* |

---

## Phase 6 — Rounding Out

### Matrix / Simulation
| # | Problem |
|---|---------|
| 48 | Rotate Image |
| 54 | Spiral Matrix |
| 73 | Set Matrix Zeroes |
| 289 | Game of Life |

### Bit Manipulation
| # | Problem |
|---|---------|
| 136 | Single Number (XOR) |
| 191 | Number of 1 Bits |
| 338 | Counting Bits |
| 190 | Reverse Bits |
| 371 | Sum of Two Integers |

### Design
| # | Problem |
|---|---------|
| 146 | LRU Cache *(if you only do one design problem, do this)* |
| 380 | Insert Delete GetRandom O(1) |
| 460 | LFU Cache (Hard) |

---

## Company patterns (rough, and they shift)

- **Meta:** speed and volume. Heavy on 1, 200, 56, 253, 236, 314 (Binary Tree Vertical Order), plus tagged-question repetition. Practice with a timer.
- **Amazon:** graphs/BFS, heaps, intervals, and ~50% of the loop is Leadership Principles behavioral. 200, 973, 253, 146.
- **Google:** the least "list-able" — expect novel problems and follow-up extensions. Strongest emphasis on clean reasoning aloud and DP/graphs.
- **Microsoft:** classic and fair. Linked lists, trees, strings. 206, 297, 42.
- **Apple / Netflix / Uber / Airbnb:** mostly standard mediums plus domain-flavored design.
- **Startups & mid-size:** usually easier algorithmically, more practical — string parsing, hash maps, sometimes a small real-world API task instead. Phases 1–3 cover most of it.

---

## The 20 highest-frequency, if you're short on time

1, 20, 121, 206, 226, 104, 200, 53, 141, 242, 3, 49, 56, 33, 102, 322, 15, 238, 146, 215

---

## Sequencing suggestion

| Weeks | Focus |
|---|---|
| 0 | Phase 0 — Python + Big-O |
| 1–2 | Phase 1 — arrays, hashing, two pointers |
| 3–4 | Phase 2 — sliding window, stack, binary search |
| 5–6 | Phase 3 — linked lists, trees |
| 7–9 | Phase 4 — heaps, backtracking, graphs |
| 10–12 | Phase 5 — DP, greedy, intervals |
| 13+ | Phase 6 + mock interviews + revisit weak spots |

Slower is fine. Depth on 100 problems beats skimming 400.

---

## Things that separate a pass from a fail

These matter as much as the algorithm:

- **Ask clarifying questions first.** Empty input? Duplicates? Negative numbers? Sorted? This is graded.
- **Talk through the brute force before optimizing.** "The naive approach is O(n²) because... let me see if a hash map gets that to O(n)." Silence reads as being stuck.
- **State complexity unprompted.**
- **Test your code out loud** on a small example, line by line. Catching your own off-by-one is a strong signal.
- **Being stuck is fine; being silent isn't.** "I'm considering two approaches and I'm unsure which handles duplicates better" is a good interview moment.
