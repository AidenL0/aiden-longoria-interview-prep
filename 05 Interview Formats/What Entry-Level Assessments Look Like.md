---
title: What Entry-Level Assessments Look Like
tags: [interview-formats, assessments]
---

# What Entry-Level Assessments Look Like

Back to [[Start Here]] · Related: [[Reading an Unfamiliar Codebase]]

## What it is

After you apply, many employers send an **online assessment** (OA): a timed test on a platform such as HackerRank, CodeSignal or Qualified. It's often the first technical step, before any interview with a person. This note describes the three formats I've actually met as an entry-level applicant, plus one common format I haven't faced yet. It describes formats only: no companies, no real questions.

## Why it matters

Prepping for the wrong format wastes weeks. Grinding puzzle problems helps less if the test asks you to fix someone else's code, and vice versa. Read the invite email closely: it usually names the platform, the language options and the time limit.

## The three formats I've met

### 1. Puzzle problems (LeetCode-style)

- **What it looks like:** one to three problems with a short description, examples, and hidden test cases that run when you submit. Often 60–90 minutes in total.
- **Topics that showed up:** trees given as a list of edges (solved with breadth-first search), and arrays.
- **How it's graded:** mostly by the hidden tests, including large inputs that catch slow solutions.
- **How to prepare:** the `02 Patterns` folder and the [[Python Roadmap]]. Practice building a tree or graph from an edge list; many problems start that way.

### 2. Code reading: find the bug, then add a feature

- **What it looks like:** a small existing program (a handful of classes) with a short document describing them. Fix one or more bugs, then add a feature.
- **The kind of bug to expect:** small logic slips rather than crashes. The usual suspects are listed in [[Reading an Unfamiliar Codebase]].
- **How it's graded:** whether the tests pass, and often how readable and well-placed your change is.
- **How to prepare:** [[Reading an Unfamiliar Codebase]]. Small exercises that give you a class with stubs and tests (like Exercism) are close practice.

### 3. Debugging a codebase (part of a puzzle test)

- **What it looks like:** paired with a puzzle problem in the same test. A small project with failing tests; you fix bugs until the tests pass.
- **How it differs from format 2:** there's no new feature, and the bugs are spread across several files.
- **How to prepare:** run the tests first, read each failure (expected versus actual), and trace the smallest failing input by hand. Same method as format 2.

## A common format I haven't met yet: SQL

SQL comes up often in postings for data and backend roles, so it's worth preparing for. I haven't taken an SQL assessment myself; this section is what I'm preparing for, not a report from one.

- **What it usually looks like:** a few database tables and questions to answer with queries.
- **What it usually tests:** `SELECT` with `WHERE`, `JOIN` across tables, `GROUP BY` with counts or sums, sorting, and sometimes a subquery.
- **How to prepare:** practice joins and grouping until you can write them without looking anything up. Read the table descriptions slowly; joining on the wrong column is an easy mistake.

## Habits that help in every format

- **Read the whole prompt first,** including the examples, before writing anything.
- **Write a one-line comment with your approach.** Some platforms let reviewers replay your typing; a comment counts as communication.
- **Watch the clock but don't rush the reading.** Misunderstanding the ask costs more than reading slowly.
- **Write down what was tested** right after each assessment (format and topics, never the exact question). Over time, that list tells you what to study.
