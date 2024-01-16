---
layout: post
title: Advanced Programming - Topic 2 - Object Oriented Programming
description: Course notes for Advanced Programming, week 2.
image: https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401091711194.jpg
category:
  - 课程笔记
tags:
  - java
toc: true
comments: true
math: false
mermaid: false
pin: false
sitemap: true
published: true
---

## Procedural vs OOP

- Procedural works well with small problems
  - e.g. renaming a set of files, simple mathematical calculations, etc.
- OOP is more suitable for larger problems
  - e.g. web browser: a few thousand procedures
  - each manipulating a set of global data
  - it is much more tractable to define classes (address bar, page contents, etc.)
  - then define methods per class (~20)
  - easier to develop, debug, and manage code written this way

### Example - Loan

- All data properties and methods are tied to a specific instance
- Class is built around the data
- Has implementation, but hidden from the outside (use)
- No-arg constructor
- “Regular” constructor
- Getters and setters (not shown)

## Main Object Oriented Principles

### Inheritance

### Encapsulation

### Abstraction

All I need to know is how to trigger it, but how it does is none of my problem.

### Polymorphism

#### Visibility General Rules

- `Constructor` is package-visible by default, but commonly public
- `Data field` is recommended to be private
- `Method` can vary
  - public: if they are part of the class's interface, you are

#### Method Overloading

Method overloads when more than one method has the `same name` but `different parameters`. The distinction is identified at compile-time by matching number and type of parameters, the compiler will identify which method to call (overloading resolution).

#### Method Overriding

Method overwriting happens when a method from the child class has **EXACTLY** the `same name` and `same parameters` and `return type` as a method in parent class. The distinction is identified at run-time by the type of object that invokes the method.
