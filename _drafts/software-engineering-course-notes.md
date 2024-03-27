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
mermaid: true
pin: false
sitemap: true
published: true
---

## Week 1 - Intro; Semester 1 Recap; OOP Basics

## Week 2 - Challenges in SE; Coupling and Cohesion

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

### Software Engineering Challenges

### Developer Roles

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

## Week 3 - Advanced OOP: Controlling Coupling/Cohesion

### Why Do We Need to Reduce Coupling?

- we don't fully remove; we want to reduce "bad" coupling
- too much coupling can indicate `poor design`
  - sharing too much information
  - tricky to use interactions (e.g. routine coupling)

Essentially, code with too much coupling is `difficult to change`.

- difficult meaning "propagates through large sections of your program"
- not a big issue when the code base is small, but massive if you have a 1 million code base.

#### Remember

Coupling and Cohesion are also about interaction/relationships.

Objects are communication cohesive: components operating on the same data are kept together. The data are instance variables, "components"(here) are methods.

Objects aren't cohesive just because they are objects. You have to make sure there isn't:

- redundant data

```java
public class Everything {
  private Database db;
  private WebsiteServer server;
  private BankAccount acc1, acc2;
  private HTMLFormatter fmt;
  ...
}
```

- methods that don't need to know about particular data

```java
public class User {
  private DataBase db;
  ...

  public String getName();
  public void validateUser();
  ...

  // Assume only this method uses db
  public void syncDB();
}
```

> This is a hint that db should not be a part of User. Have a UserDatabaseController or similar class.

### When to use Abstract Classes vs Interfaces

`abstract class`:

- when defining shared functionality among related classes.
  - e.g. default implementations of methods.
- when need to define data members in the class.
- when don't need to support multiple inheritance.

`interface`:

- Only need methods with no default implementation.
- Need to support multiple inheritance.

### Polymorphism

- object behaviour depends on the specific type, but we aren’t allowed to know the specific type!
- key way to reduce (content) coupling:
  - code against a super-class (often an interface).
  - specific implementation is unknown, which means we can change it if needed.
- can sometimes replace control coupling, no longer need an if(some flag), pass an object that does the “flagged” behaviour.

```java
// If functions were shared then abstract class would be better!
public interface Car {
    public String details();
}

public class FordFocus implements Car {
    public String details() {
        return "Ford Focus";
    }
}

public class HondaCivic implements Car {
    public String details() {
        return "Honda Civic";
    }
}

// Does the right thing, but doesn't rely on the type of Car
// We have *decoupled* the implementation using polymorphism!
public void printCarDetails(Car c) {
    System.out.println(c.details());
}
```

Interact with the most general type you can.

```java
// Usually Bad: Leaks implementation details
public ArrayList<Integer> f(...) {}

// Much better: Just says what sort of thing it is
// (some form of ordered collection)
public List<Integer> f(...) {}
```

#### The Important Idea

Always think how elements of classes/systems interact and are related.

- does this data need to be here?
- will these interactions make it difficult to change in future?

### Finality

Child classes can override any public/protected function from a parent. Private functions can't be overriden, but they also can't be called by the child. To fully control access we need something in the middle:

- the `final` keyword

```java
class Finality {
    public final String prettyPrint() {
        return "Final";
    }
}

class SubFinal extends Finality {
    public String prettyPrint() {
        return "Override";
    }
}
```

Output:

```console
Finality1.java:10: error: prettyPrint() in SubFinal cannot override prettyPrint() in Finality,
    public String prettyPrint() {
                     ^
    overridden method is final
```

Final can be applied elsewhere:

- final class: can't be extended
- final variable: can't be changed

It's all about controlling use/relations between components.

### Lab Notes

## Week 4 - UML

UML stands for Unified Modeling Language.

### Class Diagrams

Classes have three elements:

- A name
- Instance Variables
- Methods

| Visibility        | Symbol | Meaning           |
| ----------------- | ------ | ----------------- |
| Private           | -      | Class only        |
| Package Protected | ~      | Package Protected |
| Protected         | #      | Class/children    |
| Public            | +      | Anyone            |

```mermaid
classDiagram
    class Point {
        -int x
        -int y
        +int getX()
        +int getY()
        +int distanceTo(Point p)
    }
```

### Abstract Classes in UML

- Abstract class and method is the same as a class, but with the name in **_italics_**

```mermaid
classDiagram
    class Place {
        <<abstract>>
        -int population
        String placeName()*
    }
```

> Since I'm using Mermaid, annotations are used with `<<annotation>>` , so the diagram here is not exactly italicized, but the general rule stands.

### Aggregation (Built From)

```mermaid
classDiagram
    class Library
    class Book
    Library o-- "1..*" Book
```

- Clear diamond means _"part of"_

  - A Book is part of a Library
  - A Library _has_ a (collection of) Book(s)

- Weak dependency
  - A Book is still a book without a Library
  - Contrast to an account without a bank for example

### Composition (Strongly Built From)

```mermaid
classDiagram
    class Person
    class Hand
    class Leg
    Person *-- "2..2" Hand
```

## Week 5 - Testing

> Only stuff that is new to me are written down.

This week's lecture talks about how to test code, some general testing idea were introduced.

Such as:

- Levels of Testing
  - unit testing
  - integration testing
  - acceptance testing
- Different Levels of Rigour
- Test Cases
  - Given-When-Then
- Mock
- Test Coverage: a metric to (try to) determine “how well tested code is”, out of all lines-of-code, what percentage does the unit tests cover.

```java
int doSomething(int x) {
    if (x == 42) {
        println("Hidden feature");
        return 0;
    }
    println("Normal Path");
    return 1;
}
```

To test doSomething():

```java
void testDoSomething() {
    assert doSomething(1) == 1;
}

```

The above covers 3 of 5 statements, 60% coverage.

```java
void testDoSomething() {
    assert doSomething(1) == 1;
    assert doSomething(42) == 0;
}
```

The above covers 5 of 5 statements, 100% coverage.

> Caveat: 100% coverage does not mean **no errors** or definitely **meets specification**.

### Test Driven Development (TDD)

Testing is so fundamental it is the core of some methodologies

- Particularly in Agile: Spec Changes =⇒ Test Changes
- Means you need well designed tests

**TDD Loop**:

- Write a failing test (means you have to define the calling interface to use)
- Write the simplest code that makes the test pass
- Refactor the code to improve design
- Also known as “red-green-refactor”

#### TDD Example

**Step 1**: write a failing test

```java
public void testEmptyPasswordIsNotStrong() {
    String pass = "";
    PasswordVerifier v = new PasswordVerifier();
    assert !v.isStrong(pass);
}
```

`PasswordVerifier` doesn't exist yet, so this will fail.

**Step 2**: define a minimal working implementation.

```java
public class PasswordVerifier {
    public boolean isStrong(String pass) {
        return false;
    }
}
```

Test now compiles. It also is successful (green). No refactoring needed since it’s so simple.

So we write another test.

```java
public void testPasswordMoreThan8CharactersIStrong() {
    String pass = "12345689";
    PasswordVerifier v = new PasswordVerifier();
    assert v.isStrong(pass);
}
```

Fails, so we need to fix the code so it passes.

**Step 3**: Refactor

```java
public class PasswordVerifier {
    public boolean isStrong(String pass) {
        if (pass.length() >= 8) {
            return true;
        }
        return false;
    }
}
```

Passes, so we write another test.

**Step 4**: Repeat

Continue with the red-green-refactor until you are happy there’s enough tests to show specification is (likely) met.

## Week 6 - Error handling, Safe Classes, and Packages

### Overview

- What to do when things go wrong
  - Dealing with errors
  - Exceptions
- How to design classes with errors/specifications in mind
  - Testing great at catching errors; but even better if we stop them at source
  - What invariants classes have
- Java packages
  - How can we build larger cohesive units from classes

### Handling Errors

Assume we have an API like:

```java
public String readDataFromFile(String filepath)
```

There’s no way to read from a file that doesn't exist, This is an error condition. How can we handle this?

We can add our own calling conversions:

1. Return something sensible

```java
/**
 * readDataFromFile attempts to read all data from file as a string.
 * Returns empty string if filepath could not be read
 */
public String readDataFromFile(String filepath) {
    // implementation code here
}
```

Returns a sane default, e.g. what we would expect if the file was empty

Issue: No one flags that `filepath` was a problem. Is this too transparent?

2. Return an error value to check

```java
/**
 * readDataFromFile attempts to read all data from file as a string.
 *
 * Returns string "error" if filepath could not be read
 */
public String readDataFromFile(String filepath) {
    // implementation code here
}
```

This approach is common in languages like C and Unix systems, functions return 0 if okay, -1 if error.

Issue: can't force return value to be checked, might pass the string "error" into business logic.

3. Return a result type

Currently popular: return a special Optional/Maybe type.

```java
  public Optional<String> readDataFromFile(String filepath)
```

`Optional` represents a value that is either present, or not. You have to explicitly `get()` the value to use it.

```java
Optional<String> s = readDataFromFile(path);
if (!s.isPresent()) {
    requestNewFile();
} else {
    String n = s.get();
}
```

Benefits:

- API explicitly shows something can go wrong (not just a comment)
- Forces the user to check the value

### Erroneous Values

It’s usually much better to fail, than proceed based on bad assumptions/data. If you fail at least you know something went wrong. If you produce a result based on bad assumptions, can you trust the result? Lots of Machine Learning algorithms suffer from this lack of trust.

Sometimes you can’t afford to fail: life support controller, reactor safety software etc.

### Handling Errors: Exceptions

Language constructs that allow control flow to be changed on an error.

**Idea:**

- Erroneous code `throws` an Exception when it doesn’t know how to continue
- Program control flow stops
- The closest `exception handler` (for that error) is found
- Exception handler `catches` the exception by executing code

## Week 7 - Live Code Refactoring Example

## Week 8 - Design Patterns 1

## Week 9 - Design Patterns 2

## Week 10 - Looking Forward: What a SE should know

## Week 11 - Revision and Exam Prep with Gul + Blair
