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

## 5. Abstract Data Types and Collections

We classify all data into data _types_.

### Abstract Data Types

## 6. Stacks

### Overview

- Stack concepts
- Stack applications
- Stack ADTs: requirements, contracts
- Implementation of stacks: using arrays and linked-lists
- Stacks in the Java class library

### Stack concepts

A **stack** is a last-in-first-out sequence of elements. Elements can be **added** and **removed** only at one end (the **top** of the stack). We can **push** an element on to the stack, i.e., add it at the top of the stack. We can **pop** an element from the stack, i.e., remove it from the top of the stack. The **size** (or depth) of a stack is the number of elements it contains.

Consider a stack of books on a table:

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202402151338698.png)

It is a stack because we can add(push) and remove(pop) books only at the top.

### Stack Applications

- Interpreter (e.g., Java Virtual Machine):
  - uses a stack to contain intermediate results during evaluation of complicated expressions
  - also uses the stack to contain arguments and return addresses for method calls and returns.
- Parser (e.g., XML parser, parser in Java compiler):
  - uses a stack to contain symbols encountered during parsing of the source code.

TODO:......

## 7. Queues

### Overview

- Queue concepts
- Queue applications
- A queue ADT: requirements, contract
- Implementations of queues: using arrays and linked-lists
- Queues in the Java class library

### Queue concepts

### Queue applications

### A queue ADT: requirements, contract

### Implementations of queues

### Queues in the Java class library

## 8. Lists

## 9. Sets

### Overview

- Set concepts
- Set applications
- A set ADT: requirements, contract
- Implementations of sets: using member arrays, boolean arrays and linked-lists
- Sets in the Java class library

### Set concepts

A **set** is a collection of elements, with no duplicates, and
in no fixed order. An element does not have a “position” in the set.

The elements of a set are usually called its **members**.

Mathematical notation for sets:

- {a, b, …, z} is the set whose members are a, b, …, z. (The members can be written in any order.)
- {} is the empty set.

> Note: We can use this notation in algorithms, but it is not supported by Java.

## 10. Binary Search Trees
