---
title: CSharp Roadmap
tags: [roadmap, csharp]
---

# CSharp Roadmap

Back to [[Start Here]] · Main track: [[Python Roadmap]] · Syntax: [[CSharp for Python Programmers]]

**What this is:** a short C# track for when a job or an assessment uses C# (or .NET). It follows the same pattern-first order as the [[Python Roadmap]]. The algorithms are the same ones; what this trains is **translation**: writing correct C# from memory.

**The core idea:** reading C# is easy for a Python programmer; writing it from a blank page is not. Only writing closes that gap, so this roadmap is problems, not reading.

---

## How to use this list

**Per problem:**
1. Restate the problem out loud. Name the pattern and its trigger, the same as in Python.
2. Write the Python approach as a comment first. The algorithm is the solved part.
3. Translate to C#. When it won't compile, give yourself **10 minutes** with your notes and the Microsoft docs before asking for help.
4. Scan compiler output for the word `error`, not `warning`.
5. State the time and space complexity before moving on.
6. Keep a **snag list**: one line for every compile or runtime surprise. Review it before each session. ([[CSharp for Python Programmers]] has a starter list.)
7. **Rewrite from blank** once it passes. If you can't, you haven't learned it yet.

**AI help:** useful early on if it explains syntax instead of writing the solution. In the last days before a test, switch to documentation only, which matches test conditions.

**Spaced repetition:** redo anything you needed help with after 3 days, then 10.

---

## Phase 0: the C# toolkit

**Know cold:**

```csharp
// The method shape: every problem
public class Solution {
    public int[] TwoSum(int[] nums, int target) {
        // ...
        return new int[] { -1, -1 };   // every path must return something
    }
}

// Return an array (Python: return [i, j])
return new int[] { i, j };

// Frequency map (Python: counts[c] = counts.get(c, 0) + 1)
var counts = new Dictionary<char, int>();
foreach (char c in s) {
    counts[c] = counts.GetValueOrDefault(c) + 1;
}

// Letter counts with an array (Python: [0] * 26 and ord(c) - ord('a'))
int[] freq = new int[26];
foreach (char c in s) {
    freq[c - 'a']++;
}

// "if key in dict" and read it, in one step
if (seen.TryGetValue(complement, out int j)) {
    return new int[] { j, i };
}

// Set: Add returns false on a duplicate, no error
var seen = new HashSet<int>();
if (!seen.Add(x)) { /* x was already there */ }

// Slice (Python: s[i:j]). The second number is a LENGTH
string part = s.Substring(i, j - i);

// Build a string: never += in a loop
var sb = new StringBuilder();
sb.Append(c);
string result = sb.ToString();

// Sentinel (Python: float('inf'))
int best = int.MaxValue;
```

**Length versus Count:** arrays and strings use `.Length`. `List`, `Dictionary`, `HashSet`, `Stack` and `Queue` use `.Count`.

---

## Phase 1: twelve problems

Each one drills a specific piece of C#. Most are on the Python roadmap too, so only the translation is new.

### Arrays and hashing

| # | Problem | Pattern | C# skill it drills |
|---|---|---|---|
| 412 | [[412 Fizz Buzz - CSharp]] | Simulation | Method shape, `%`, returning `IList<string>` |
| 1 | [[1 Two Sum - CSharp]] | Hash map complement | `Dictionary`, `TryGetValue`, `return new int[] { }` |
| 217 | [[217 Contains Duplicate - CSharp]] | Seen-before set | `HashSet.Add` returning `false` |
| 242 | [[242 Valid Anagram - CSharp]] | Frequency counting | `int[26]` with `c - 'a'` |
| 26 | Remove Duplicates from Sorted Array | Slow/fast pointer | In-place array edits, returning a count |

### Strings

| # | Problem | Pattern | C# skill it drills |
|---|---|---|---|
| 125 | [[125 Valid Palindrome - CSharp]] | Converging pointers | `char.IsLetterOrDigit`, `char.ToLower` |
| 344 | [[344 Reverse String - CSharp]] | Converging pointers | In-place `char[]` swap: `(s[i], s[j]) = (s[j], s[i])` |
| 13 | [[13 Roman to Integer - CSharp]] | Lookup table | `Dictionary<char, int>` initializer |
| 14 | [[14 Longest Common Prefix - CSharp]] | Vertical scan | Nested loops over strings, building a string |

### Stack, linked list, geometry

| # | Problem | Pattern | C# skill it drills |
|---|---|---|---|
| 20 | [[20 Valid Parentheses - CSharp]] | Stack matching | `Stack<char>`, `Peek`, `Count > 0` before popping |
| 206 | [[206 Reverse Linked List - CSharp]] | Pointer reversal | `ListNode`, `null` checks |
| 836 | [[836 Rectangle Overlap - CSharp]] | Non-overlap test | Boolean logic on coordinates |

**Checkpoint:** you can write the method shape, a `Dictionary` lookup and an array return without looking anything up.

**Rewrite from blank:** Two Sum two days after first solving it; 217 and 242 a few days later.

---

## Phase 2: test conditions

- **Timed mock:** three unseen easy problems in 30 minutes, documentation only (pick from 268, 283, 88, 121, 704).
- Fix whatever the mock exposed, then practice reading tests: [Exercism's C# track](https://exercism.org/tracks/csharp) hands you a class with stubs and tests.
- **Day before:** review the snag list, nothing new.
- **Test day:** close every AI tool. Keep only the Microsoft docs open.

---

## C# templates for later phases

**Binary search:**
```csharp
int lo = 0, hi = arr.Length - 1;
while (lo <= hi) {
    int mid = lo + (hi - lo) / 2;      // avoids int overflow
    if (arr[mid] == target) return mid;
    else if (arr[mid] < target) lo = mid + 1;
    else hi = mid - 1;
}
return -1;
```

**Reverse a linked list:**
```csharp
ListNode prev = null, curr = head;
while (curr != null) {
    ListNode next = curr.next;   // save the rest of the list
    curr.next = prev;            // flip the pointer
    prev = curr;                 // step forward
    curr = next;
}
return prev;
```

**DFS on a tree:**
```csharp
int Dfs(TreeNode node) {
    if (node == null) return 0;
    int left = Dfs(node.left), right = Dfs(node.right);
    return 1 + Math.Max(left, right);
}
```

**BFS on a tree:**
```csharp
var q = new Queue<TreeNode>();
q.Enqueue(root);
while (q.Count > 0) {
    int levelSize = q.Count;          // snapshot = one level
    for (int i = 0; i < levelSize; i++) {
        TreeNode node = q.Dequeue();
        if (node.left != null) q.Enqueue(node.left);
        if (node.right != null) q.Enqueue(node.right);
    }
}
```

**Backtracking:**
```csharp
void Backtrack(int start, List<int> path) {
    res.Add(new List<int>(path));     // copy! not path
    for (int i = start; i < nums.Length; i++) {
        path.Add(nums[i]);
        Backtrack(i + 1, path);
        path.RemoveAt(path.Count - 1); // undo
    }
}
```

**Sliding-window ladder** (sorting with a partner, then a window, then both): [[2418 Sort the People - CSharp]] → [[219 Contains Duplicate II - CSharp]] → [[904 Fruit Into Baskets - CSharp]] → [[2747 Count Zero Request Servers - CSharp]].

---

## After a C# test

Go back to the [[Python Roadmap]] as the main track. Later interview rounds are often whiteboard problems graded on process, so practice problems like 206 and 836 out loud: approach first, then complexity, then a small test.

---

## Things that separate a pass from a fail

Everything in the [[Python Roadmap]] still applies. C# adds:

- **Read the error code before guessing.** CS0161: a path doesn't return. CS1061: a wrong member name, like `.Length` on a List or a lowercase method. CS1002: a missing semicolon.
- **Comment your intent.** On many assessment platforms reviewers read your code, and some can replay how you typed it. A one-line comment showing your approach counts as communication.
- **Prefer checking over crashing.** Use `ContainsKey`, `Count > 0` and `!= null` before reading. It's clearer to a reviewer than a try/catch.
