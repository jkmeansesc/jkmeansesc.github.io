---
layout: post
title: Advanced Programming Course Notes
description: Course note for Advanced Programming
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
date: 2024-01-27 01:55 +0800
---
## Topic 1 - Course Intro & Java Basics

### Understand How Methods Work

**Examples**

```java
class PrimeFinder {
  static boolean isPrime(int n) {
    for (int x = 2; x <= (int) Math.sqrt(n); x++) {
      if (n % x == 0) {
        return false; // early termination
      }
    }
    return true; // not divisible by any number <= sqrt(n)
  }

  public static void main(String[] args) {
    if (args.length < 1) {
      System.out.println("usage: java PrimeFinder <max-range>");
      return;
    }
    int maxRange = Integer.parseInt(args[0]);
    for (int i = 2; i <= maxRange; i++) {
      if (isPrime(i)) {
        System.out.println(i);
      }
    }
  }
}
```

Write a program to out put the following patterns

```console
*
**
***
****
*****
```

```java
    int i, j;
    final int N = 5;
    for (i = 1; i <= N; i++) {
      for (j = 1; j <= i; j++) System.out.print('*');
      System.out.println();
    }
```

### Understand Passing by Value/Reference

**Pass by value**

```java
  public static void tripleValue(double x) {
    x = 3 * x;
  }

  public static void main(String[] args) {
    double percent = 10;
    System.out.println("Before: percent=" + percent); // 10
    // When the method ends, the parameter variable x is no longer in use.
    // Meanwhile percent has not been touched because only value of percent is passed.
    tripleValue(percent);
    System.out.println("After: percent=" + percent); // still 10
  }
```

**Pass by reference**

- `Correct example`

```java
public class callbyref_modify {
  public static void raiseSalary(Employee x, double byPercent) {
    double salary = x.getSalary();
    double raise = salary * byPercent / 100;
    x.setSalary(salary + raise);
  }
  public static void main(String[] args) {
    var harry = new Employee("Harry", 30000);
    System.out.println("Before: salary=" + harry.getSalary()); // 30000
    // the value of location address of harry is passed
    // the method modifies the salary of the object of that location address
    // when the method ends, x is no longer in use
    // but harry continues to refer to the object whose salary was increased
    raiseSalary(harry, 10);
    System.out.println("After: salary=" + harry.getSalary()); //so the output is 33000
  }
}

class Employee {
  private String name;
  private double salary;
  public Employee(String n, double s) {
    name = n;
    salary = s;
  }
  public double getSalary() {
    return salary;
  }
  public void setSalary(double newSalary) {
    this.salary = newSalary;
  }
}
```

- `Incorrect example`

```java
public class callbyref_swap {
  public static void swap(Employee x, Employee y) {
    Employee temp = x;
    x = y;
    y = temp;
  }

  public static void main(String[] args) {
    var a = new Employee("Alice", 70000);
    var b = new Employee("Bob", 60000);
    System.out.println("Before: a=" + a.getName()); // Alice
    System.out.println("Before: b=" + b.getName()); // Bob
    // x and y are initialized with copies of the references to a and b
    // the method swaps the copies, but not a and b
    // when the method ends, x and y are no longer in use
    // and the original variables a and b still refer to the same objects
    swap(a, b);
    System.out.println("After: a=" + a.getName()); // Alice
    System.out.println("After: b=" + b.getName()); // Bob
  }
}

class Employee {
  private String name;
  private double salary;

  public Employee(String n, double s) {
    name = n;
    salary = s;
  }

  public String getName() {
    return name;
  }
}
```

In summary:

Both primitive types and objects are **passed by value**, not the variable/object itself.

This means:

- a method can use but not modify a parameter of a primitive type.
- a method cannot modify the reference of an object.
  - e.g. make it refer to a new object
- a method CAN modify the state of an object.
  - e.g. change the value of an instance variable

### Understand What is a Class

A class:

- describes a set of objects with the same behaviour
- allows generalizing the types of variables
- models data types:

  - basic types, e.g. int, double, etc.
  - abstract data types, hence multiple levels of abstraction

Usage

- Definition: defines the building blocks of the class (type)
- Instantiation: creates a physical instance in memory of the abstract data type

```java
/**
 * This class is used to describe a point in 2-dimensional space.
 * It has three methods: add, scale, and minus.
 * The add method is used to add two points together.
 * The scale method is used to scale a point by a factor.
 * The minus method is used to subtract two points.
 */
class Vec2d {
  float x;
  float y;

  public Vec2d(float x, float y) {
    this.x = x;
    this.y = y;
  }

  public Vec2d add(Vec2d that) {
    return new Vec2d(this.x + that.x, this.y + that.y);
  }

  public Vec2d scale(float alpha) {
    return new Vec2d(alpha * this.x, alpha * this.y);
  }

  public Vec2d minus(Vec2d that) {
    Vec2d temp = that.scale(-1.0f);
    return this.add(temp);
  }
}
```

### Inheritance

- Models “is a” relations between types
  - e.g. a Square is a Rectangle
  - Square = derived/inherited/subclass; Rectangle = base/super class
- Useful to reuse functionalities already implemented in the base class
  - e.g. area() method of Square can reuse that of Rectangle
- Base classes can be used to implement generic functions common to all subtypes
  - Code to compute perimeter of a 4 sided polygon can be implemented inside a class Quadrilateral
  - No repetition of the functionality inside each individual quadrilateral type, like rhombus, rectangle, parallelogram, etc.
  - More specialized functions, such as area computation, can be performed by each special quadrilateral type

```java
public class Quadrilateral {
  Vec2d a, b, c, d;
  Quadrilateral(Vec2d a, Vec2d b, Vec2d c, Vec2d d) {
    this.a = a;
    this.b = b;
    this.c = c;
    this.d = d;
  }
  float perimeter() {
    // logic to calculate the perimeter
  }
}

class Parallelogram extends Quadrilateral {
  Parallelogram(Vec2d a, Vec2d b, Vec2d c, Vec2d d) {
    // reuse the super-class constructor
    super(a, b, c, d);
  }
  float area() {
    // logic to calculate the area
  }
}

class Rectangle extends Parallelogram {
  // ...and so on
}
```

### Interfaces

- Defines a contract that every class that follows should adhere to
  - ≅ a template of methods that every subclass should implement
- e.g. If an application wants to enforce that each Quadrilateral object
  must involve a strict type check (say, check that the 4 points are
  distinct), this could be specified as an interface

```java
interface ConstrainedPolygon {

  boolean isValid(); // each subclass MUST implement this method
}

class Parallelogram extends _4gon implements ConstrainedPolygon {
  // member variables and methods go here
  public boolean isValid() {
    // check if this is a valid parallelogram
  }
}

class Rectangle extends Parallelogram {
  // member variables and methods go here
  public boolean isValid() {
    // override (i.e. same signature but different logic),
    // as not every parallelogram is a rectangle
  }
}
```

### Abstract Classes

- An abstract class cannot be instantiated; only extended
- They define types/concepts that are abstract
  - do not have enough information to be realized in a stand-alone manner
    - e.g. a Shape is too abstract to exist on its own
  - they sit at the roots of class hierarchies (hence, they have to be public)
- Used to group together classes that should have common functions of different implementations (i.e. abstract methods)
  - e.g. checkIfInside for the Shape abstract class -- checking if a given point is within the shape once it is actually defined
- They can also contain normal methods and variables

```java
public abstract class Shape {
  public abstract boolean checkIfInside(Vec2d a);
  // no implementation!
  // all extending classes MUST implement this
}
```

### Interfaces vs. Abstract Classes

- Both cannot be instantiated
- Both can inherit/extend others of their own type
- Both contain abstract methods
  - Interfaces only contain abstract methods
  - hence no need for the abstract keyword
- Abstract classes define a common type, a concept
  - contains (implemented / non-implemented) logic and variables
  - this will be inherited, creating a hierarchy of types
- Interfaces define expected behaviour
  - notionally a finer type of contract
  - a class can implement more than one interface

## Topic 2 - Object Oriented Programming

### Overview

**Aim**
Explain the main concepts of Object Oriented Programming.

**Contents**

- Definition
- Main principles
- Key Characteristics of Objects
- Constructors
- Accessor and Mutator Methods
- The final (key)word

### What is OOP?

Object-oriented programming (OOP) is a programming paradigm that focuses on objects. As the name suggests, in OOP programs, data is stored as objects. It not only defines how to store data but also specifies the logic, or methods, that can be applied to the data.

Object-oriented programming (OOP) is older than Java. Other object-oriented programming languages include Simula, Smalltalk, C++, Objective-C, Ruby, and JavaScript. It replaced procedural programming languages such as Fortran, Pascal, C, and others. In procedural programming, the focus is on procedures and algorithms, with data coming second, it means that it treats data and procedures as interconnected and inseparable entities.

### Procedural vs OOP

Procedural approaches work effectively for small-scale issues, such as renaming a collection of files or performing simple mathematical calculations.

Object-oriented programming (OOP) is more suitable for larger problems, such as developing a web browser. For instance, a web browser involves several thousand procedures, each manipulating a set of global data. In such complex scenarios, it is more tractable to define classes (e.g., the address bar, page contents, etc.). Subsequently, defining methods per class (approximately 20) becomes easier. This approach facilitates easier development, debugging, and code management.

With the object-oriented (OOP) approach, each object holds specific data and functions. In other words, an object comprises both data and related methods. The implementation of the functionality is concealed, but the means to trigger it are exposed. A class defines how objects are created; it serves as a template, with classes being likened to cookie cutters, and objects or instances being the resulting cookies.

**Example - Loan**

All data properties and methods are tied to a specific instance. The class is built around the data. It has an implementation, hidden from external use. There is a no-argument constructor. A constructor with no arguments is also available. Getters and setters are included (not shown).

```java
public class Loan {
  private double annualInterestRate;
  private int numberOfYears;
  private double loanAmount;
  private java.util.Date loanDate;

  /** Default constructor */
  public Loan() {
    this(2.5, 1, 1000);
  }

  /** Construct a loan with specified annual interest rate, number of years and loan amount */
  public Loan(double annualInterestRate, int numberOfYears, double loanAmount) {
    this.annualInterestRate = annualInterestRate;
    this.numberOfYears = numberOfYears;
    this.loanAmount = loanAmount;
    loanDate = new java.util.Date();
  }

  /** Return annualInterestRate */
  public double getAnnualInterestRate() {
    return annualInterestRate;
  }

  /** Set a new annualInterestRate */
  public void setAnnualInterestRate(double annualInterestRate) {
    this.annualInterestRate = annualInterestRate;
  }

  /** Return numberOfYears */
  public int getNumberOfYears() {
    return numberOfYears;
  }

  /** Set a new numberOfYears */
  public void setNumberOfYears(int numberOfYears) {
    this.numberOfYears = numberOfYears;
  }

  /** Return loanAmount */
  public double getLoanAmount() {
    return loanAmount;
  }

  /** Set a newloanAmount */
  public void setLoanAmount(double loanAmount) {
    this.loanAmount = loanAmount;
  }

  /** Find monthly payment */
  public double getMonthlyPayment() {
    double monthlyInterestRate = annualInterestRate / 1200;
    double monthlyPayment =
        loanAmount
            * monthlyInterestRate
            / (1 - (Math.pow(1 / (1 + monthlyInterestRate), numberOfYears * 12)));
    return monthlyPayment;
  }

  /** Find total payment */
  public double getTotalPayment() {
    double totalPayment = getMonthlyPayment() * numberOfYears * 12;
    return totalPayment;
  }

  /** Return loan date */
  public java.util.Date getLoanDate() {
    return loanDate;
  }
}
```

External use is facilitated through publicly exposed methods. All data and implementation are encapsulated.

```java
import java.util.Scanner;

public class LoanTestClass {
  /** Main method */
  public static void main(String[] args) {
    // Create a Scanner
    Scanner input = new Scanner(System.in);

    // Enter yearly interest rate
    System.out.print("Enter yearly interest rate, for example, 8.25: ");
    double annualInterestRate = input.nextDouble();

    // Enter number of years
    System.out.print("Enter number of years as an integer: ");
    int numberOfYears = input.nextInt();

    // Enter loan amount
    System.out.print("Enter loan amount, for example, 120000.95: ");
    double loanAmount = input.nextDouble();

    // Create Loan object
    Loan loan = new Loan(annualInterestRate, numberOfYears, loanAmount);

    // Display loan date, monthly payment, and total payment
    System.out.printf(
        "The loan was created on %s\n" + "The monthly payment is %.2f\nThe total payment is %.2f\n",
        loan.getLoanDate().toString(), loan.getMonthlyPayment(), loan.getTotalPayment());
  }
}
```

### Main Object Oriented Principles

#### Inheritance

Creating relationships between data types (classes) is achieved through inheritance. A subclass inherits all the properties and methods of its superclass, enabling code reuse, reducing redundancy and inconsistent logic, and allowing for a hierarchy of types. More general classes (super classes or abstract classes) are positioned at the top of the hierarchy, while more specific classes reside lower down. Implicitly, all Java classes inherit from the "cosmic superclass" Object.

#### Encapsulation

Encapsulation refers to the result of combining data and behavior in a single unit, which is a class. By retaining the state and implementation of classes, we protect the data (encapsulate it) within the class. The state can only change through method calls, making public methods the gatekeepers. Methods never directly access instance fields in a class other than their own.

Define what data is made accessible: Control how data is manipulated – an object is considered a "black box".

Prevent data corruption and ensure reliability.

> I only need to know how to trigger it; understanding how it works is not my problem.

#### Abstraction

Abstraction separates implementation from usage, concealing implementation details, and exposes only ways to invoke it.

```java
class Employee {
  private String name;

  public String getName() {
    return name;
  }
}
```

Key to reuse: underlying implementation can change

```java
class Employee {
  private String firstName;
  private String lastName;

  public String getName() {
    return firstName + " " + lastName;
  }
}
```

The change is invisible to the remainder of the program.

#### Polymorphism

Objects can assume more than one form depending on the context. Methods can have identical signatures but function slightly differently.

Two forms of polymorphism exist:

1. Overloading (compile-time or static polymorphism): writing methods with similar signatures but distinct parameters.
2. Overriding (runtime or dynamic polymorphism): replacing existing methods with identical signatures.

### Object Characteristics

An object is an entity with 3 key characteristics

- `Behaviour` – what can you do with it? What methods can you apply to it?
- `State` – how does it react when you apply methods?
- `Identity` – how is it different from others like it?

How these characteristics differ

- `Behaviour` – same for all objects of the same class (same methods)
- `State` – usually different between objects of the same class, only changes through method calls (encapsulation)
- `Identity` – always different between objects even if their state is the same, e.g. 2 identical orders on Amazon are still distinct

### Constructors

Constructors are special methods used to create new objects in Java. They are always called using the new keyword. The name of a constructor is always the same as that of the class. A constructor has no return value and can have zero or more parameters. The constructor of the superclass (if any) must be called first using the `super()` keyword. A class can have multiple constructors, each with a unique signature. Java does not support destructors since it automatically performs garbage collection.

### Accessor and Mutator Methods

An accessor (getter) method retrieves the value of a class variable. A mutator (setter) method modifies the value or reference of a class variable. In their simplest form, they serve these distinct purposes. However, in reality, they offer additional functionalities:

1. Access control: controlling who can read and modify variables
2. Validation: ensuring data consistency and integrity
3. Consistency guarantees: maintaining an expected state of the object
4. Error checking: detecting and handling potential issues

The benefits are significant:

1. Encapsulation: hiding the internal representation, allowing the interface to remain constant
2. Alternative: directly exposing class variables with no control

#### Visibility General Rules

Constructors: by default, package-visible but commonly marked as public

Data fields: recommended to be private

Methods: can vary in visibility

- Public: if they are part of a class's interface
  - Expected to keep the method available regardless of version changes
- Private/protected: mainly for internal use (helper methods)
  - Easier to change or even discontinue

#### Method Overloading

Method overloading occurs when multiple methods have `the same name` but `different parameter lists`. The distinction between these methods is determined at compile-time through a process called overloading resolution, during which the compiler identifies the most suitable method based on the number and types of the provided parameters.

```java
public class MethodOverloading {
  /** Main method */
  public static void main(String[] args) {
    // Invoke the compare method with int parameters
    System.out.println("compare(3,4) is " + compare(3, 4));

    // Invoke the compare method with the double parameters
    System.out.println("compare(3.0,4.0) is " + compare(3.0, 4.0));

    // Invoke the compare method with three double parameters
    System.out.println("compare(2.5,5.4,10.4) is " + compare(2.5, 5.4, 10.4));
  }

  /** Return the larger of two int values */
  public static int compare(int num1, int num2) {
    if (num1 > num2) return num1;
    else return num2;
  }

  /** Return the smaller of two double values */
  public static double compare(double num1, double num2) {
    if (num1 < num2) return num1;
    else return num2;
  }

  /** Return the minimum among three double values */
  public static double compare(double num1, double num2, double num3) {
    return compare(compare(num1, num2), num3);
  }
}
```

A common example is constructor overloading:

```java
public class Pet {
  protected String name;
  protected int age;
  public Pet (String name, int age) {
    this.name = name;
    this.age = age;
  }
  public Pet (String name) {
    this(name, 0);
  }
}
```

#### Method Overriding

Method overriding occurs when a method in a child class has the `same name`, `identical parameters`, and the `same return type` as a method in the parent class. This distinction is determined at runtime based on the object's type that invokes the method.

```java
class Employee {
  private String name;
  private double salary;
  private String department;

  public Employee (String name, double salary, String department) {
    this.name = name;
    this.salary = salary;
    this.department = department;
  }

  public void setName (String name) {
    this.name = name;
  }

  public double getSalary () {
    return salary;
  }
}

class Manager extends Employee {
  private double bonus;

  public Manager (String name, double salary, String department, double bonus) {
    super(name, salary, department);
    this.bonus = bonus;
  }

  public double getSalary () {
    return super.getSalary() + bonus;
  }
}
```

### Polymorphic Object Variables

A variable of type X can refer to an object of class X or its subclasses.

```java
  Manager boss = new Manager("John", 50000, "Sales", 5000);
  Employee[] staff = new Employee[3];
  staff[0] = boss;
```

The variables `staff[0]` and boss refer to the same object, but `staff[0]` is considered to be only an Employee object by the compiler. So:

```java
  boss.setBonus(10000); // OK
  staff[0].setBonus(10000); // Error
```

`setBonus` is not a method of the Employee class. The opposite is not possible. You cannot assign a superclass reference to a subclass variable.

### The final Keyword

Use the final keyword in a class definition to indicate that it cannot be extended (overridden).

```java
  public final class Executive extends Manager {}
```

One good reason to use the "final" keyword is that it can be used with methods and classes, preventing overriding in subclasses. This ensures that the semantics of the class or method cannot be changed in a subclass. As the superclass developer, you take full responsibility for the implementation, and no subclass should be allowed to modify it.

## Topic 3 - Design Patterns

### Overview

**Aim**

Introduce some high-level design considerations that help create
flexible designs that are easy to maintain and can cope with change

**Contents**

- Design Principles: Aggregation / Composition
- Design Patterns
  - Composite
  - Decorator
  - Observer

### Design Principles

1. As your architectural design grows, (1) relationships will multiply, (2) relationships will be more complicated , (3) classes will diversify with more corner cases, (4) code will evolve.

2. OO principles (inheritance, polymorphism, etc.) will not suffice, (1) code reuse will be more challenging, (2) code will be duplicated with minimal differences, (3) changes could be difficult to manage.

3. Code maintenance deteriorates as design grows / changes which will inevitably happen.

### Design Principles – Class Design

Once you have an initial OO design, you should aim to achieve:

- `Cohesion` – a class describes a single concept and its methods logically fit to support a coherent purpose or high-level responsibility.
- `Clarity` – easy-to-explain classes, methods, and variables.
- `Consistency` – naming convention, informative naming, overload methods of
  similar operations, follow standard practice (e.g. define a no-arg constructor).
- `Access` – use encapsulation and getters+setters where suitable.
- `Appropriate member declaration` – dependent on specific instance, or shared by all instances (static).
- `Inheritance` – “is a” relationship (as opposed to “has a”).

### Design Principles – Aggregation

The aggregation (sometimes composition) design principle

- separate the parts that **vary** from those that do not / definitely will not
- define contracts (APIs)
- develop code to use contracts to integrate the variable parts

Result

- classes have a one-way form of association rather than inheriting
- “has a” relationship is added dynamically where&when needed
- behaviour can be changed at runtime
- implementation is independent from design
- maximising code reuse and separation of concerns

#### Example

A web server can compress images in different ways, having one compression method is restricting. Instead, create the interface

```java
public interface CompressAlgorithm {
  byte [] compress(byte[]);
}
```

that is implemented by classes such as:

```java
public class ZlibCompression impletements CompressAlgorithm {
  public byte [] compress(byte[]) {
    // code to compress using the zlib structure
  }
}
```

and instantiate from this or other classes that implement the
`CompressAlgorithm` interface (“has a relationship”). This way, you are changing behaviour at runtime rather than inheriting only one behaviour at design time.

### Design Patterns

#### The Composite Pattern

In some applications we might need to perform the same operation on objects or groups of objects such as collections of objects to store state: array, hash table, etc. The composite pattern allows you to compose objects into hierarchies, and treat individual objects or compositions of them in a uniform manner.

For example:

- online store – applying discount rates on individual or multi-pack items
- e-commerce – aggregating nested inventories from providers using different systems
- file system – computing the size of files and folders
- GUI – all components from the top-level down perform the same operation, e.g. click

**Main elements:**

- `Component`

Component is the highest level of abstraction, typically an interface. It defines all methods we want to be able to invoke on objects / groups of objects.

- doOperation() – key logic
- add(), remove() – group management
- getChildren(), getParent() – hierarchy traversa

```java
public interface ShopComponent {
  public Double compPrice(Double discount);
}
```

- `Leaf`

Leaf is a class for individual objects in the system that implements the operation and hierarchy traversal methods defined in the component.

```java
public class ShopLeaf implements ShopComponent {
  private Double basePrice;
  private Boolean canBeDiscounted;
  private String name;

  // Constructor - set the properties of the item
  public ShopLeaf(Double base, Boolean disc, String n) {
    basePrice = base;
    canBeDiscounted = disc;
    name = n;
  }

  // Comp price - only discounts if discounting is allowed
  public Double compPrice(Double discount) {
    if (canBeDiscounted) {
      return basePrice * (1.0 - (discount / 100.0));
    } else {
      return basePrice;
    }
  }

  // Nice display
  public String toString() {
    return name;
  }
}
```

- `Composite`

Composite is a class for groups of objects. It implements everything defined in the Component interface, including group management logic.

```java
import java.util.ArrayList;

public class ShopComposite implements ShopComponent {
  // This will store the leaves
  private ArrayList<ShopComponent> children;
  private String name;

  // Constructor - create the list and set the name
  public ShopComposite(String n) {
    children = new ArrayList<ShopComponent>();
    name = n;
  }

  // Composites normally delegate the methods to the leaves
  public Double compPrice(Double discount) {
    Double price = 0.0;
    // arraylists can be iterated...
    for (ShopComponent a : children) {
      price += a.compPrice(discount);
    }
    return price;
  }

  // Add and remove just call the arraylist methods
  public void add(ShopComponent a) {
    children.add(a);
  }

  public void remove(ShopComponent a) {
    children.remove(a);
  }

  // A nice toString method that displays the composite name
  // and the children names
  public String toString() {
    String totalString = name;
    totalString += " {";
    for (ShopComponent a : children) {
      // Invokes toString on children..
      totalString += a + ",";
    }
    totalString += "} collection";
    return totalString;
  }
}
```

#### The Decorator Pattern

The Decorator Pattern adjusts the behavior of an object without modifying its code. It composes complex behaviors without changing the interface or sub classes. It is indeed more like encapsulation.

**Main elements:**

- `Component`

```java
public abstract class Beverage {
  String description = "Unknown Beverage";

  public String getDescription() {
    return description;
  }

  public abstract double cost();
}
```

- `Concrete Component`

```java
public class DarkRoast extends Beverage {
  public DarkRoast() {
    description = "Dark Roast Coffee";
  }

  public double cost() {
    return .99;
  }
}

public class Decaf extends Beverage {
  public Decaf() {
    description = "Decaf Coffee";
  }

  public double cost() {
    return 1.05;
  }
}

public class Espresso extends Beverage {
  public Espresso() {
    description = "Espresso";
  }

  public double cost() {
    return 1.99;
  }
}

public class HouseBlend extends Beverage {
  public HouseBlend() {
    description = "House Blend Coffee";
  }

  public double cost() {
    return .89;
  }
}
```

- `Decorator`

```java
public abstract class CondimentDecorator extends Beverage {
  public abstract String getDescription();
}
```

- `Concrete Decorators`

```java
public class Milk extends CondimentDecorator {
  Beverage beverage;

  public Milk(Beverage beverage) {
    this.beverage = beverage;
  }

  public String getDescription() {
    return beverage.getDescription() + ", Milk";
  }

  public double cost() {
    return .10 + beverage.cost();
  }
}

public class Mocha extends CondimentDecorator {
  Beverage beverage;

  public Mocha(Beverage beverage) {
    this.beverage = beverage;
  }

  public String getDescription() {
    return beverage.getDescription() + ", Mocha";
  }

  public double cost() {
    return .20 + beverage.cost();
  }
}

public class Soy extends CondimentDecorator {
  Beverage beverage;

  public Soy(Beverage beverage) {
    this.beverage = beverage;
  }

  public String getDescription() {
    return beverage.getDescription() + ", Soy";
  }

  public double cost() {
    return .15 + beverage.cost();
  }
}

public class Whip extends CondimentDecorator {
  Beverage beverage;

  public Whip(Beverage beverage) {
    this.beverage = beverage;
  }

  public String getDescription() {
    return beverage.getDescription() + ", Whip";
  }

  public double cost() {
    return .10 + beverage.cost();
  }
}
```

Finally, we can test it:

```java

public class StarbuzzCoffee {

  public static void main(String args[]) {
    Beverage beverage = new Espresso();
    System.out.println(beverage.getDescription() + " $" + beverage.cost());

    // Make a DarkRoast object
    Beverage beverage2 = new DarkRoast();
    // Wrap it with a Mocha.
    beverage2 = new Mocha(beverage2);
    // Wrap it in a second Mocha
    beverage2 = new Mocha(beverage2);
    // Wrap it in a Whip
    beverage2 = new Whip(beverage2);
    System.out.println(beverage2.getDescription() + " $" + beverage2.cost());

    Beverage beverage3 = new HouseBlend();
    beverage3 = new Soy(beverage3);
    beverage3 = new Mocha(beverage3);
    beverage3 = new Whip(beverage3);
    System.out.println(beverage3.getDescription() + " $" + beverage3.cost());
  }
}
```

#### The Observer Pattern

The Observer Pattern helps keep your objects up-to-date about information they care about. It is useful when your program has a class (the Subject) containing some kind of state that might be required by various other classes. The observer ensures that they are all updated whenever the subject is updated using a publish-subscribe approach.

The Observer Pattern is used on pretty much any event-based system, such as GUIs, social networks, e-commerce transactions, etc.

**Main elements:**

- `The Subject` – the class that contains (and publishes) the state

```java
import java.util.ArrayList;

public class Subject {
  private Double[] data;
  private ArrayList<Observer> observers = new ArrayList<Observer>();

  public Double[] getData() {
    return data;
  }

  public void setData(Double[] data) {
    this.data = data;
    this.notifyAllObservers();
  }

  public void attach(Observer observer) {
    observers.add(observer);
  }

  private void notifyAllObservers() {
    for (Observer observer : observers) {
      observer.notifyMe();
    }
  }
}
```

- `The Observer` – an abstract class for objects that can subscribe to state updates

```java
public abstract class Observer {
  protected Subject subject;

  public abstract void notifyMe();
}
```

- `Concrete Observers` – any class that extends the Observer in order to be able to subscribe and unsubscribe to state updates

```java
public class ListDataObserver extends Observer {
  public ListDataObserver(Subject subject) {
    this.subject = subject;
    this.subject.attach(this);
  }

  public void notifyMe() {
    Double[] data = subject.getData();
    System.out.println();
    System.out.println("Here's the data:");
    for (int i = 0; i < data.length; i++) {
      System.out.println(data[i]);
    }
  }
}

public class MeanDataObserver extends Observer {
  public MeanDataObserver(Subject subject) {
    this.subject = subject;
    this.subject.attach(this);
  }

  public void notifyMe() {
    Double[] data = subject.getData();
    Double mean = 0.0;
    for (int i = 0; i < data.length; i++) {
      mean += data[i];
    }
    mean = mean / (1.0 * data.length);
    System.out.println();
    System.out.println("Mean: " + mean);
  }
}
```

Finally, test it:

```java
public class ObserverTest {
  public static void main(String[] args) {
    Subject s = new Subject();
    Double[] d = new Double[5];
    d[0] = 1.0;
    d[1] = 1.2;
    d[2] = 1.4;
    d[3] = 1.7;
    d[4] = 2.4;
    new ListDataObserver(s);
    new MeanDataObserver(s);
    s.setData(d);
    d[3] = 3.2;
    s.setData(d);
  }
}
```
