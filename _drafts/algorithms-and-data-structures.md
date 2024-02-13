---
layout: post
title: Algorithms and Data Structures Course Notes
description: Course notes for Algorithms and Data Structures
image: https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401111658932.jpg
category:
  - 课程笔记
tags:
  - algorithm
toc: true
comments: true
math: true
mermaid: false
pin: false
sitemap: true
published: true
---

## Week 1

## Week 2

Principles

The algorithm must be:

- expressed in some language that the processor "understands"
- solvable

Efficiency analysis

Given a choice of algorithms for the same problem, which one is the best?

Depends on:

- How much time does it require?

We want the algorithm that grows slowly in time taken to execute.

- How much space (memory) does it requrie?
- Is it feasible or efficient enough?

How should we measure time?

Example: Smart Power Algorithm
Idea: $b^{1000} = b^{500} \times  b^{500}$. If we know $b&{500}$, we can compute $b^{1000}$ with only 1 more multiplication!
Smart power algorithm:
To compute bn: 1. Set p to 1, set q to b, and set m to n.2. While m > 0, repeat: 2.1. If m is odd, multiply p by q. 2.2. Halve m (discarding any remainder).  2.3. Multiply q by itself.3. Terminate yielding p.

$2^{17}$

### Recursive Algorithms

A `recursive algorithm` is an algorithm in which at least one step that calls itself.

- \+ more elegant and easier to understand
- \- less efficient (extra calls consumes time and space)

## Week 3

### Array Data Structure

### Properties

## Week 4 - Linked Lists

### Overview

- Linked-lists: singly-linked-lists, doubly-linked-lists
- Insertion
- Deletion
- Searching

### Linked-lists

A **linked-list** consists of a `header` together with a sequence of `nodes` connected by links. Each node (except the last) has a `next node`. Each node (except the first) has a `predecessor node`. Each node contains a single element (value or object), plus links to its next and/or predecessor.
