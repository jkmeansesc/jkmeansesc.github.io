---
layout: post
title: Software Engineering Course Notes
description:
image:
category:
  - 课程笔记
tags:
  - course
toc: true
comments: true
math: false
mermaid: false
pin: false
sitemap: true
published: true
---

## Software Engineering Challenges

Challenge 1: Scale

- No way any one person can fully understand all that code

  - Research suggest that around 10000 lines (of well designed code) max

- Necessitates:
  - Techniques to deal with complexity
  - Working in teams

Challenge 2: Change

The agile techniques are all about change

Abstraction as a Fundamental Technique to handle Scale

Example:

- Using List over ArrayList
  - List is a general "thing"
- ArrayList has much more detail

## Developer Roles

Code-Owner: The person (or team) developing a particular component.

Code-User: The person (or team) using a particular component.

Don't need to know everything, an abstract model of what happens is enough.

### Ownership

Important to be aware of ownership

### Coupling and Cohesion

Q: How do we best split up a program into component?
Q: What makes a good component?

We want high cohesion and low coupling.

High Cohesion:

- Internas of components should be related.

  - e.g. you don't expect the database connector to be inside the HTML rendering function

- Components should do one thing well

Low Coupling:
Internal details should not leak between components, if they do, `change` becomes very difficult.
