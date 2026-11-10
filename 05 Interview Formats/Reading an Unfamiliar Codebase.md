---
title: Reading an Unfamiliar Codebase
tags: [interview-formats, code-reading, debugging]
---

# Reading an Unfamiliar Codebase

Back to [[Start Here]] · Related: [[What Entry-Level Assessments Look Like]], [[CSharp for Python Programmers]]

## What it is

A method for assessments (and real jobs) where you're handed existing code and asked to fix a bug or add a feature. A **codebase** is just the collection of files that make up a program.

## Why it matters

In this format, the hard part isn't writing an algorithm. It's two skills:

1. **Understanding the ask:** turning a written description, and types you've never seen, into "given this input, I return this output."
2. **Navigating the code:** knowing which class owns which data and behavior, so new code goes in the right place and matches what's around it.

It's easy to spend most of the time on these without noticing. Budget for them on purpose.

## The method

Before writing any code (spend roughly the first fifth of your time here):

1. **Map the classes.** One line per class: what it holds (its fields) → what it does (its methods) → who uses it. Draw arrows for "creates" and "calls."
2. **Find the entry point:** the method the task or the tests call. Follow one call all the way through.
3. **Pin down the custom types.** For each type you don't know, find its constructor, its fields and one place it's used. Then build one tiny example of it by hand.
4. **Turn the ask into an example.** Write one concrete input and the exact output expected. If you can't, you don't understand the ask yet: reread the document or the existing tests before coding.
5. **Bugs: reproduce, then trace.** Make the wrong output happen, then follow the values on that input with a **trace table** (one column per variable, one row per step). Look for a value that should change but doesn't, or one that changes when it shouldn't.
6. **Features: find the closest existing behavior.** New code usually belongs in the class that owns the data it uses, next to a method that does something similar. Copy that method's shape: naming, return style, error handling.
7. **Change as little as possible.** Add code rather than restructuring, and match the existing style even where you'd write it differently.

## The usual suspects for bugs

- Off by one in a loop bound (`<` versus `<=`).
- A variable declared or reset **inside** a loop when it should keep its value between passes.
- A missing `return`, or a `return` that ends a loop on its first pass.
- A `null` that isn't checked.
- Integer division where a decimal result was needed.
- A substring or slice with the wrong length or end index.

## Reading test output

Most C# tests use NUnit; Python uses `unittest` or `pytest`. A failure names the test, the **expected** value and the **actual** value (`Expected: 5  But was: 4`). If an exception was thrown instead, the first line of the stack trace that points into the code you were given is where to look.

## Practice

- Ask a friend (or an AI tool) to write 4–5 small classes for one feature, with a planted bug and a feature request, small enough to finish in under an hour. Work it timed, then review where the time went.
- Exercism exercises hand you a class with stubs and tests: practice for fitting code into an existing structure.
- Reading teammates' code in a class project and adding to it is the same skill.
