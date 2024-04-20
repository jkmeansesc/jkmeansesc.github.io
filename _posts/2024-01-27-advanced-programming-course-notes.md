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
mermaid: true
pin: false
sitemap: true
published: true
date: 2024-01-27 01:55 +0800
---

## Topic 1: course intro & java basics

### Methods

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

### Passing by value/reference

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

Java pass both primitive types and objects **by value**, not the variable/object itself.

This means:

- a method can use but not modify a parameter of a primitive type.
- a method can't modify the reference of an object.
  - for example: make it refer to a new object
- a method CAN modify the state of an object.
  - for example: change the value of an instance variable

### Class

A class:

- describes a set of objects with the same behaviour
- allows generalizing the types of variables
- models data types:

  - basic types, for example: int, double, etc.
  - abstract data types, hence multiple levels of abstraction

Usage

- Definition: defines the building blocks of the class
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
  - for example: a Square is a Rectangle
  - Square = derived/inherited/subclass; Rectangle = base/super class
- Useful to reuse functionalities already implemented in the base class
  - for example: area() method of Square can reuse that of Rectangle
- Base classes can implement generic functions common to all subtypes
  - code to compute perimeter of a 4 sided polygon inside a class Quadrilateral
  - no repetition of the capability inside each individual quadrilateral type, like rhombus, rectangle, parallelogram, etc.
  - each special quadrilateral type can perform more specialized functions, such as area computation.

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
- for example: if an app needs to enforce that each Quadrilateral object undergoes a strict type verification, say, verifying that the four points are distinct, you can specify this requirement in an interface.

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

### Abstract classes

- An abstract class can't be instantiated; only extended
- They define types/concepts that are abstract
  - do not have enough information to be realized in a stand-alone manner
    - for example: a Shape is too abstract to exist on its own
  - they sit at the roots of class hierarchies, hence, they have to be public
- Used to group together classes that should have common functions of different implementations: abstract methods
  - for example: `checkIfInside` for the Shape abstract class -- checking if a given point is within the shape once it's actually defined
- They can also contain normal methods and variables

```java
public abstract class Shape {
  public abstract boolean checkIfInside(Vec2d a);
  // no implementation!
  // all extending classes MUST implement this
}
```

### Interfaces compares to abstract classes

- Both can't be instantiated
- Both can inherit/extend others of their own type
- Both contain abstract methods
  - Interfaces only contain abstract methods
  - hence no need for the abstract keyword
- Abstract classes define a common type, a concept
  - contains implemented / non-implemented logic and variables
  - inherited, creating a hierarchy of types
- Interfaces define expected behaviour
  - notionally a finer type of contract
  - a class can implement more than one interface

### Topic 1 Lab

What you will do in this lab:

- Design and inherit classes
- Make your own abstract class and interface - Refactor classes to construct class hierarchies

### Exercise 1a: Compiling

This initial exercise gives you some practice of compiling and running code from the command line.

- Open a text editor and write a short Java programme with a main method that prints
  Hello World.
- Save this is as a file called HelloWorld.java
- Open a command prompt
- Navigate to the folder containing the file you just created
- Compile the code using: `javac HelloWorld.java`, if successful, this will create a HelloWorld.class file)
- Run the code using: `java HelloWorld` Note: java not javac, and no extension after the filename.

```java
  public class HelloWorld {
    public static void main(String[] args) {
      System.out.println("Hello World");
    }
  }
```

### Exercise 1b: Designing classes

Write a new Java file that has a class to represent an employee. This class is to be used for staff management purposes. The class requires to:

- Be called Employee
- Have 4 instance variables: name, ID, salary, department
- Have a constructor that takes 3 parameters to match the instance variables except ID - Have a method called `editName` to modify the employee’s name. Make sure that you use appropriate variable types and visibility modifiers.

```java
class Employee {
  private String name;
  private int ID;
  private double salary;
  private String department;

  public Employee(String n, double s, String d) {
    this.name = n;
    this.salary = s;
    this.department = d;
    numOfEmployees++;
    this.ID = numOfEmployees;
  }

  public void editName(String newName) {
    this.name = newName;
  }

  public double getSalary() {
    return this.salary;
  }
}
```

### Exercise 1c: Static

Create a static variable within the Employee class to hold the value of the number of employees created. Each time an Employee instance is created, ID is set according to the value of this variable which is then incremented.

Next, create a static method that prints the number of unique IDs that have been assigned to employees.

```java
class Employee {
  ...
  private static int numOfEmployees = 0;
  public static int getIDcount() {
    return numOfEmployees;
  }
}
```

### Exercise 1d: Inheritance

Design a Manager class that inherits from Employee. The subclass needs to have an additional instance variable to hold the value of a bonus (surplus to salary). The subclass also needs to have a method called `calcTotalEarnings` to return the total earnings of the manager as the sum of the salary and the bonus. Also, implement a new constructor that initialises a new instance using the constructor of the superclass. Satisfy yourself that you have edited the appropriate methods.

```java
class Manager extends Employee {
  private double bonus;

  public Manager(String n, double s, String d, double b) {
    super(n, s, d);
    this.bonus = b;
  }

  public double calcTotalEarnings() {
    return getSalary() + bonus;
  }
}
```

### Exercise 1e: Abstract classes

Design an abstract class Person for defining a person with a name. Go back and edit your Employee class to inherit the Person abstract class.

```java
abstract class Person {
  String name;
}

class Employee extends Person {
  ...
}
```

### Exercise 1f: Interfaces

Design a new class Invoice to represent an invoice, with instance variables to hold the value of the invoice and a textual description.

Now create an interface Payable that defines a method `calcPaymentAmount` which returns the value to be paid for a given class the implements the new interface. Make both the Employee and Invoice classes implement this interface.

```java
interface Payable {
  public double calcPaymentAmount();
}

class Invoice implements Payable {
  double value;
  String description;

  public double calcPaymentAmount() {
    return this.value;
  }
}

class Employee extends Person implements Payable {
  ...
  public double calcPaymentAmount() {
    return this.salary;
  }
}
```

## Topic 2: object oriented programming

### Overview

**Aim**
Explain the main concepts of Object Oriented Programming.

**Contents**

- Definition
- Main principles
- Key Characteristics of Objects
- Constructors
- Accessor and Mutator Methods
- The final keyword

### OOP?

Object-oriented programming (OOP) is a programming paradigm that focuses on objects. As the name suggests, in OOP programs, data is stored as objects. It not only defines how to store data but also specifies the logic, or methods, that can be applied to the data.

Object-oriented programming (OOP) is older than Java. Other object-oriented programming languages include Simula, Smalltalk, C++, Objective-C, Ruby, and JavaScript. It replaced procedural programming languages such as Fortran, Pascal, C, and others. In procedural programming, the focus is on procedures and algorithms, with data coming second, it means that it treats data and procedures as interconnected and inseparable entities.

### Procedural compares to OOP

Procedural approaches work effectively for small-scale issues, such as renaming a collection of files or performing simple mathematical calculations.

Object-oriented programming (OOP) is more suitable for larger problems, such as developing a web browser. For instance, a web browser involves several thousand procedures, each manipulating a set of global data. In such complex scenarios, it's more tractable to define classes, for example:, the address bar, page contents, etc. Subsequently, defining methods per class approximately 20 becomes easier. This approach facilitates easier development, debugging, and code management.

With the OOP approach, each object holds specific data and functions. In other words, an object comprises both data and related methods. The implementation of the capabilities is concealed, but the means to trigger it are exposed. A class defines how objects are created, it serves as a template, with classes being likened to cookie cutters, and objects or instances being the resulting cookies.

**Example - Loan**

All data properties and methods are tied to a specific instance. The class is built around the data. It has an implementation, hidden from external use. There is a no-argument constructor. A constructor with no arguments is also available. Getters and setters are included .

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

### Main object oriented principles

#### Inheritance

Creating relationships between data types is achieved through inheritance. A subclass inherits all the properties and methods of its superclass, enabling code reuse, reducing redundancy and inconsistent logic, and allowing for a hierarchy of types. More general classes, super classes or abstract classes, are positioned at the top of the hierarchy, while more specific classes reside lower down. Implicitly, all Java classes inherit from the "cosmic superclass" Object.

#### Encapsulation

Encapsulation involves combining data and behavior within a single unit, known as a class. This process protects the data by retaining the state and implementation details inside the class. The state can only change through method calls, positioning public methods as the gatekeepers. Methods avoid directly accessing instance fields in a class other than their own.

Define what data is made accessible: control how data is manipulated, an object is considered a "black box."

Prevent data corruption and ensure reliability.

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

1. Overloading: writing methods with similar signatures but distinct parameters.
2. Overriding: replacing existing methods with identical signatures.

### Object characteristics

An object is an entity with 3 key characteristics

- `Behaviour`: what can you do with it? What methods can you apply to it?
- `State`: how does it react when you apply methods?
- `Identity`: how's it different from others like it?

How these characteristics differ

- `Behaviour`: same for all objects of the same class.
- `State`: usually different between objects of the same class, only changes through method calls, this is encapsulation.
- `Identity`: always different between objects even if their state is the same, for example: 2 identical orders on Amazon are still distinct

### Constructors

Constructors are special methods used to create new objects in Java. They're always called using the new keyword. The name of a constructor is always the same as that of the class. A constructor has no return value and can have zero or more parameters. The constructor of the superclass, if any, must be called first using the `super()` keyword. A class can have multiple constructors, each with a unique signature. Java doesn't support destructors since it automatically performs garbage collection.

### Accessor and Mutator methods

An Accessor, getter, method retrieves the value of a class variable. A Mutator, setter, method modifies the value or reference of a class variable. In their simplest form, they serve these distinct purposes. However, in reality, they offer additional functionalities:

1. Access control: controlling who can read and modify variables
2. Validation: ensuring data consistency and integrity
3. Consistency guarantees: maintaining an expected state of the object
4. Error checking: detecting and handling potential issues

The benefits are significant:

1. Encapsulation: hiding the internal representation, allowing the interface to remain constant
2. Alternative: directly exposing class variables with no control

#### Visibility general rules

Constructors: by default, package-visible but commonly marked as public

Data fields: recommended to be private

Methods: can vary in visibility

- Public: if they're part of a class's interface
  - Expected to keep the method available regardless of version changes
- Private/protected: mainly for internal use as helper methods
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

#### Method overriding

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

### Polymorphic object variables

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

`setBonus` isn't a method of the Employee class. The opposite isn't possible. You can't assign a superclass reference to a subclass variable.

### The final keyword

Use the final keyword in a class definition to indicate that it can't be overridden.

```java
  public final class Executive extends Manager {}
```

One good reason to use the "final" keyword is that it can be used with methods and classes, preventing overriding in subclasses. This ensures that the semantics of the class or method can't be changed in a subclass. As the superclass developer, you take full responsibility for the implementation, and no subclass should be allowed to modify it.

### Topic 2 Lab

In Lab 1, we explored inheritance. This lab delves deeper into inheritance and introduces other object-oriented concepts, notably encapsulation and polymorphism.

#### Exercise 2a: Inheritance

- **Objective:** Define a class `Pet` that extends the abstract class `AbstractPet`. Consider if any methods need definition in `Pet`.
  - **Question:** Are there any methods that need definition in `Pet`? Why or why not?

```java
public abstract class AbstractPet {
  protected String name;
  protected int age;

  public AbstractPet(String n, int a) {
    name = n;
    age = a;
  }

  public String toString() {
    return name + " is aged " + age;
  }
}
```

There's no need to define extra methods because the parent class `AbstractPet` already defined them.

#### Exercise 2b: Constructors

- **Objective:** Implement two constructors for `Pet`:
  - A constructor with two parameters.
  - A constructor with one parameter, initializing the name and setting age to 0.

```java
public class Pet extends AbstractPet {

  public Pet(String name, int age) {
    super(name, age);
  }

  public Pet(String name) {
    super(name, 0);
  }
  ...
}

```

#### Exercise 2c: Getters and Setters

- **Objective:** Define getter and setter methods for the instance variables of the `Pet` class.
  - Ensure the name is a non-empty string and the age is a positive integer through error checking in the setters.

```java
public class Pet extends AbstractPet {
  ...
  public String getName() {
    return super.name;
  }

  public int getAge() {
    return super.age;
  }

  public void setName(String name) {
    if (name.isEmpty()) {
      throw new IllegalArgumentException("Name cannot be empty");
    }
    super.name = name;
  }

  public void setAge(int age) {
    if (age < 0) {
      throw new IllegalArgumentException("Age cannot be negative");
    }
    super.age = age;
  }
  ...
}
```

#### Exercise 2d: Method Overriding

- **Objective:** Create a `toString` method in `Pet` to print `name + " is my pet and is aged " + age`.
  - **Questions:**
    - Which method is this overriding?
    - The `toString` method in `AbstractPet` is itself overriding another. Which is it?

```java
public class Pet extends AbstractPet {
  ...
  // Exercise 2d: this method overrides the toString method from the parent class
  // AbstractPet
  // AbstractPet's toString method overrides the toString method of Object
  @Override
  public String toString() {
    return super.name + " is my pet and is aged " + super.age;
  }
}
```

#### Exercise 2e: Inheritance II

- **Objective:** Extend the `Pet` class with two new classes: `Cat` and `Dog`. Add suitable instance variables for breed and furColour.
  - Specialize `Cat` with a `favouriteSpot` and `Dog` with a `favouriteToy`.
  - Add a `giveTreat` method to `Dog` that prints `name + " says thanks for the treat!"`.

```java
/** Exercise 2e */
public class Cat extends Pet {

  private String breed;
  private String furColor;
  private String favouriteSpot;

  public Cat(String name, int age, String breed, String furColor, String favouriteSpot) {
    super(name, age);
    this.breed = breed;
    this.furColor = furColor;
    this.favouriteSpot = favouriteSpot;
  }

  public String getBreed() {
    return breed;
  }

  public void setBreed(String breed) {
    this.breed = breed;
  }

  public String getFurColor() {
    return furColor;
  }

  public void setFurColor(String furColor) {
    this.furColor = furColor;
  }

  public String getFavouriteSpot() {
    return favouriteSpot;
  }

  public void setFavouriteSpot(String favouriteSpot) {
    this.favouriteSpot = favouriteSpot;
  }

  // Exercise 2f
  public String toString(String extra) {
    return super.name
        + " is a "
        + this.getBreed()
        + " and enjoys sleeping on "
        + this.getFavouriteSpot()
        + extra;
  }
}

/** Exercise 2e */
public class Dog extends Pet {
  private String breed;
  private String furColor;
  private String favouriteToy;

  public Dog(String name, int age, String breed, String furColor, String favouriteToy) {
    super(name, age);
    this.breed = breed;
    this.furColor = furColor;
    this.favouriteToy = favouriteToy;
  }

  public String getBreed() {
    return breed;
  }

  public void setBreed(String breed) {
    this.breed = breed;
  }

  public String getFurColor() {
    return furColor;
  }

  public void setFurColor(String furColor) {
    this.furColor = furColor;
  }

  public String getFavouriteToy() {
    return favouriteToy;
  }

  public void setFavouriteToy(String favouriteToy) {
    this.favouriteToy = favouriteToy;
  }

  public void giveTreat() {
    System.out.println(super.name + " says thanks for the treat!");
  }
}
```

#### Exercise 2f: Method Overloading

- **Objective:** In both `Cat` and `Dog`, overload the `toString` method to include an extra parameter and an additional sentence about the pet.
  - Example: `"Rex is a collie and enjoys playing with a tennis ball every day."`

```java
public class Cat extends Pet {
  ...
  // Exercise 2f
  public String toString(String extra) {
    return super.name
        + " is a "
        + this.getBreed()
        + " and enjoys sleeping on "
        + this.getFavouriteSpot()
        + extra;
  }
}

public class Dog extends Pet {
  ...
  // Exercise 2f
  public String toString(String extra) {
    return super.name
        + " is a "
        + this.getBreed()
        + " and enjoys playing with "
        + this.getFavouriteToy()
        + extra;
  }
}
```

#### Exercise 2g: Testing

- **Objective:** Build a `PetTest` class to test the defined classes.
  - Create a `Pet` array to hold objects of types `Pet`, `Cat`, and `Dog`, instantiated with suitable values.
  - Invoke `toString` on each array element and discuss which variants of the method are called and why.
  - Pick an array element holding a `Dog` instance and invoke `provideTreat` on it. Discuss the outcome.

```java
/**
 * Exercise 2g: Cat's toString calls the cat's toString method, because it is passed with an extra string that is only defined in the Cat class. Without the extra string, it will invoke the toString method of the parent class Pet. The same goes for Dog.
 */
public class PetTest {
  public static void main(String[] args) {

    Pet[] pets = new Pet[5];
    pets[0] = new Dog("Fido", 5, "Labrador", "Black", "Ball");
    pets[1] = new Cat("Mittens", 3, "Persian", "White", "The couch");
    pets[2] = new Dog("Rex", 1, "German Shepherd", "Brown", "Stick");
    pets[3] = new Cat("Snowball", 2, "Siamese", "White", "The bed");
    pets[4] = new Pet("Yoyo", 2);

    for (Pet p : pets) {
      if (p instanceof Dog) {
        System.out.println(((Dog) p).toString(" every day"));
        ((Dog) p).giveTreat();
      } else if (p instanceof Cat) {
        System.out.println(((Cat) p).toString(" every day"));
      } else {
        System.out.println(p);
      }
    }
  }
}
```

**Output**:

```console
Fido is a Labrador and enjoys playing with Ball every day
Fido says thanks for the treat!
Mittens is a Persian and enjoys sleeping on The couch every day
Rex is a German Shepherd and enjoys playing with Stick every day
Rex says thanks for the treat!
Snowball is a Siamese and enjoys sleeping on The bed every day
Yoyo is my pet and is aged 2
```

## Topic 3: design patterns

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

### Design principles

As your architectural design grows:

- relationships multiply
- relationships becomes more complicated
- classes diversify with more corner cases
- code evolves.

Therefore, OO principles like inheritance, polymorphism, etc. no longer suffice:

- code reuse becomes more challenging
- code can be duplicated with minimal differences
- changes could be difficult to manage.

Code maintenance deteriorates as design grows / changes which inevitably happen.

#### Class design

Once you have an initial OO design, you should aim to achieve:

- `Cohesion`: a class describes a single concept and its methods logically fit to support a coherent purpose or high-level responsibility.
- `Clarity`: easy-to-explain classes, methods, and variables.
- `Consistency`: naming convention, informative naming, overload methods of similar operations, follow standard practice, for example: define a no-arg constructor.
- `Access`: use encapsulation and Getters and Setters where suitable.
- `Appropriate member declaration`: dependent on specific instance, or shared by all static instances.
- `Inheritance`: “is a” relationship, as opposed to “has a”.

#### Aggregation

The aggregation, sometimes composition, design principle

- separate the parts that **vary** from those that don't / definitely won't
- define contracts
- develop code to use contracts to integrate the variable parts

Result

- classes have a one-way form of association rather than inheriting
- “has a” relationship is added dynamically where&when needed
- behaviour can be changed at runtime
- implementation is independent from design
- maximising code reuse and separation of concerns

**Example**:

A web server can compress images in different ways, having one compression method is restricting. Instead, create the interface

```java
public interface CompressAlgorithm {
  byte [] compress(byte[]);
}
```

That's implemented by classes such as:

```java
public class ZlibCompression impletements CompressAlgorithm {
  public byte [] compress(byte[]) {
    // code to compress using the zlib structure
  }
}
```

And instantiate from this or other classes that implement the `CompressAlgorithm` interface “has a relationship”. This way, you are changing behaviour at runtime rather than inheriting only one behaviour at design time.

### Design patterns

#### The composite pattern

In some applications there's a need to perform the same operation on objects or groups of objects such as collections of objects to store state: array, hash table, etc. The composite pattern allows you to compose objects into hierarchies, and treat individual objects or compositions of them in a uniform manner.

For example:

- online store: applying discount rates on individual or multi-pack items
- e-commerce: aggregating nested inventories from providers using different systems
- file system: computing the size of files and folders
- GUI: all components from the top-level down perform the same operation, for example: click

**Main elements:**

- `Component`

Component is the highest level of abstraction, typically an interface. It defines all methods that are able to be invoked on objects / groups of objects.

- doOperation: key logic
- add, remove: group management
- getChildren, getParent: hierarchy traverse

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

#### The decorator pattern

The Decorator Pattern adjusts the behavior of an object without modifying its code. It composes complex behaviors without changing the interface or sub classes. It's indeed more like encapsulation.

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

Finally, test it:

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

#### The observer pattern

The Observer Pattern helps keep your objects up-to-date about information they care about. It's useful when your program has a class, the Subject, containing some kind of state that might be required by various other classes. The observer ensures that they're all updated whenever the subject is updated using a publish-subscribe approach.

The Observer Pattern is used on pretty much any event-based system, such as GUI, social networks, e-commerce transactions, etc.

**Main elements:**

- `The Subject`: the class that contains, and publishes, the state

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

- `The Observer`: an abstract class for objects that can subscribe to state updates

```java
public abstract class Observer {
  protected Subject subject;

  public abstract void notifyMe();
}
```

- `Concrete Observers`: any class that extends the Observer to be able to subscribe and unsubscribe to state updates

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

### Topic 3 Lab

This lab focuses on applying design patterns to solve common software engineering problems. Specifically, we will explore the Composite and Decorator design patterns.

#### Exercise 3a: Composite Design Pattern

- **Overview:** Following the shop discount calculation example from the lecture, this exercise involves enhancing the existing implementation.
- **Objective:** Implement a new method `compDelivery()` as declared in the updated component. Determine where this method should be implemented. When invoked, it should return 5% of the total value of the items if they're eligible for a discount, or £0 if not.
- **Test Case:** Utilize the provided test driver class to validate your implementation. The expected output is:

```plaintext
iPad Air costs 30.0 to deliver.
MacBook costs 42.5 to deliver.
Power Adaptor costs 0.0 to deliver.
Magic Keyboard costs 0.0 to deliver.
Magic Trackpad costs 0.0 to deliver.
Mac accessory pack {Power Adaptor, Magic Keyboard, Magic Trackpad,} collection costs 0.0 to deliver.
MacBook pack {MacBook, Power Adaptor, Magic Keyboard, Magic Trackpad,} collection costs 42.5 to deliver.
```

**ShopComponent.java**:

```java
public interface ShopComponent {
  public Double compPrice(Double discount);

  public Double compDelivery();
}
```

**ShopComposite.java**:

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

  // new method
  public Double compDelivery() {
    Double deliveryCharge = 0.0;
    for (ShopComponent a : children) {
      deliveryCharge += a.compDelivery();
    }
    return deliveryCharge;
  }

  // Add and remove just call the arraylist methods
  public void add(ShopComponent a) {
    children.add(a);
  }

  public void remove(ShopComponent a) {
    children.remove(a);
  }

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

**ShopLeaf.java**:

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

  // new method
  public Double compDelivery() {
    if (canBeDiscounted) {
      return 0.05 * basePrice;
    } else {
      return 0.0;
    }
  }

  // Nice display
  public String toString() {
    return name;
  }
}
```

**CompositeDeliveryDriver.java**:

```java
public class CompositeDeliveryDriver {
  public static void displayDelivery(ShopComponent c) {
    System.out.println(c + " costs " + c.compDelivery() + " to deliver.");
  }

  public static void main(String[] args) {

    ShopLeaf ipad = new ShopLeaf(600.0, true, "iPad Air");
    ShopLeaf laptop = new ShopLeaf(850.0, true, "MacBook");
    ShopLeaf powerAdaptor = new ShopLeaf(50.0, false, "Power Adaptor");
    ShopLeaf keyboard = new ShopLeaf(125.0, false, "Magic Keyboard");
    ShopLeaf trackpad = new ShopLeaf(115.0, false, "Magic Trackpad");
    displayDelivery(ipad);
    displayDelivery(laptop);
    displayDelivery(powerAdaptor);
    displayDelivery(keyboard);
    displayDelivery(trackpad);

    ShopComposite accessoryPack = new ShopComposite("Mac accessory pack");
    accessoryPack.add(powerAdaptor);
    accessoryPack.add(keyboard);
    accessoryPack.add(trackpad);
    displayDelivery(accessoryPack);

    ShopComposite laptopPack = new ShopComposite("MacBook pack");
    laptopPack.add(laptop);
    laptopPack.add(powerAdaptor);
    laptopPack.add(keyboard);
    laptopPack.add(trackpad);
    displayDelivery(laptopPack);
  }
}
```

#### Exercise 3b: Decorator Design Pattern

- **Overview:** This exercise introduces the decorator pattern through a car customization scenario.
- **Objective:**
- Implement the `LuxuryCar` class, which hasn't been provided.
- Implement two concrete decorators: `AlloyDecorator` and `CDDecorator`. The `AlloyDecorator` adds an extra cost of £250, while the `CDDecorator` adds £150.
- **Test Case:** Verify your solution using the provided driver class. The correct output should be:

```plaintext
Polo costs 10000.0 and is a basic car
GolfSport costs 10250.0 and is a basic car + Alloys
GolfBeats costs 10150.0 and is a basic car + CD Player
SuperGolf costs 10400.0 and is a basic car + Alloys + CD Player
BMW costs 50150.0 and is a shinier, faster car than the basic one + CD Player
```

**Car.java** and **LuxuryCar.java**:

```java
public abstract class Car {
  public abstract Double getPrice();
  public abstract String getDescription();
}

public class LuxuryCar extends Car {
  public Double getPrice() {
    return 50000.0;
  }
  public String getDescription() {
    return "a shinier, faster car than the basic one";
  }
}
```

**CarDecorator.java** and **AlloyDecorator.java** and **CDDecorator.java**:

```java
public abstract class CarDecorator extends Car {
  protected Car decoratedCar;

  public CarDecorator(Car decoratedCar) {
    this.decoratedCar = decoratedCar;
  }

  public Double getPrice() {
    return decoratedCar.getPrice();
  }

  public String getDescription() {
    return decoratedCar.getDescription();
  }
}

public class AlloyDecorator extends CarDecorator {
  public AlloyDecorator(Car decoratedCar) {
    super(decoratedCar); // Call the CarDecorator constructor
  }

  public Double getPrice() {
    return super.getPrice() + 250; // Add the price of alloys
  }

  public String getDescription() {
    return super.getDescription() + " + Alloys";
  }
}

public class CDDecorator extends CarDecorator {
  public CDDecorator(Car decoratedCar) {
    super(decoratedCar); // Call the CarDecorator constructor
  }

  public Double getPrice() {
    return super.getPrice() + 150; // Add the price of alloys
  }

  public String getDescription() {
    return super.getDescription() + " + CD Player";
  }
}
```

**DecoratorTest.java**:

```java
public class DecoratorTest {
  public static void main(String[] args) {
    Car base = new BasicCar();
    System.out.println("Polo costs " + base.getPrice() + " and is " + base.getDescription());

    Car alloys = new AlloyDecorator(new BasicCar());
    System.out.println(
        "GolfSport costs " + alloys.getPrice() + " and is " + alloys.getDescription());

    Car cd = new CDDecorator(new BasicCar());
    System.out.println("GolfBeats costs " + cd.getPrice() + " and is " + cd.getDescription());

    Car all = new CDDecorator(new AlloyDecorator(new BasicCar()));
    System.out.println("SuperGolf costs " + all.getPrice() + " and is " + all.getDescription());

    Car cd2 = new CDDecorator(new LuxuryCar());
    System.out.println("BMW costs " + cd2.getPrice() + " and is " + cd2.getDescription());
  }
}
```

## Topic 4: Containers

### Overview

**Aim**

- Explore some of the rich libraries provided by Java Collections Framework
- Give examples of use for you to try yourself

**Contents**

- Collection
- Iterator
- List: ArrayList, LinkedList
- Set: HashSet, TreeSet
- Map: HashMap, TreeMap

### Collections

A collection is a container object that holds a group of objects. The Java Collections Framework supports three types of collections

- lists
- sets
- queues

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401301506590.png)

#### The collection interface

A common interface for manipulating a collection of objects.

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401301459440.png)

> see [Collection](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)

#### Iterator objects

Provide a uniform way for traversing elements in a container.

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401301507492.png)

> see [Iterable](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)

```java
import java.util.*;

public class TestIterator {
    public static void main(String[] args) {
        Collection<String> collection = new ArrayList<>();
        collection.add("New York");
        collection.add("Atlanta");
        collection.add("Dallas");
        collection.add("Madison");

        Iterator<String> iterator = collection.iterator();
        while (iterator.hasNext()) {
            System.out.print("-> ");
            System.out.print(iterator.next().toUpperCase());
        }
        System.out.println();
    }
}
```

Output:

```console
-> NEW YORK-> ATLANTA-> DALLAS-> MADISON
```

### List

Stores elements in a sequential order. The user can access the elements by using their index. The user can also specify where to store an element. It's simple to use for linear storage. You would want to linearly iterate through the container in situations such as these. Searching has a time complexity of `O(n)`, meaning the time it takes to search is directly proportional to the size of the list. This can be improved by sorting the list, reducing the time complexity to `O(log n)`. However, sorting becomes less effective when dealing with inputs that can grow dynamically, such as data being read from a stream, file, network, etc.

#### The list interface

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401301509600.png)

> see [List](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)

#### The list iterator

Additional means of traversing list containers.

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401301510414.png)

#### ArrayList and LinkedList

`ArrayList` is an option if you need to support random access through an index without inserting or removing elements from any place other than the end. For example, **_if reads are far more frequent than writes_**.

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401301515608.png)

`LinkedList` is a better choice if you require the insertion or deletion of elements from any position in the list. In contrast, **_if writes are more frequent than reads_**.

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401301516111.png)

Array is an alternative if you **_don't require the insertion or deletion of elements_**.

```java
import java.util.*;

public class TestArrayAndLinkedList {
    public static void main(String[] args) {
        List<Integer> arrayList = new ArrayList<>();
        arrayList.add(1); // 1 is autoboxed to an Integer object
        arrayList.add(2);
        arrayList.add(3);
        arrayList.add(1);
        arrayList.add(4);
        arrayList.add(0, 10);
        arrayList.add(3, 30);

        System.out.print("A list of integers in the array list: ");
        System.out.println(arrayList);

        LinkedList<Object> linkedList = new LinkedList<>(arrayList);
        linkedList.add(1, "red");
        linkedList.removeLast();
        linkedList.addFirst("green");

        System.out.print("Display the linked list forward: ");
        ListIterator<Object> listIterator = linkedList.listIterator();
        while (listIterator.hasNext()) {
            System.out.print(listIterator.next() + " ");
        }
        System.out.println();

        System.out.print("Display the linked list backward: ");
        listIterator = linkedList.listIterator(linkedList.size());
        while (listIterator.hasPrevious()) {
            System.out.print(listIterator.previous() + " ");
        }
        System.out.println();
    }
}
```

Output:

```console
A list of integers in the array list: [10, 1, 2, 30, 3, 1, 4]
Display the linked list forward: green 10 red 1 2 30 3 1
Display the linked list backward: 1 3 30 2 1 red 10 green
```

### Set

> Important: anything that implements Set can't have `duplicate` elements.

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401301530381.png)

Useful in situations where you wish to store `distinct` elements. For example, checking if an element x is prime involves a simple lookup into an existing set of primes. Sets come equipped with built-in efficient search mechanisms for identifying duplicates. There are two primary implementations:

- `Hash Sets`: An unordered collection that doesn't maintain the order in which elements are inserted. Instead, a hash function is used to determine the existence of an element.

  - only needs the equality check, for which often a byte-by-byte comparison would suffice
  - you must override `equals()` and `hashCode()`
  - not thread-safe: if multiple threads try to modify a HashSet at the same time, then the final outcome isn't deterministic

> Hash function: A deterministic, one-way mathematical function that generates a value from a given input.

```java
import java.util.*;

public class TestHashSet {
    public static void main(String[] args) {
        // Create a hash set
        Set<String> set = new HashSet<>();

        // Add strings to the set
        set.add("Monday");
        set.add("Tuesday");
        set.add("Wednesday");
        set.add("Friday");
        set.add("Friday");

        System.out.println(set);

        // Display the elements in the hash set
        for (String s : set) {
            System.out.print(s.toUpperCase() + " ");
        }
        System.out.println();

        // Process the elements using a forEach method
        set.forEach(e -> System.out.print(e.toLowerCase() + " "));
        System.out.println();
    }
}
```

Output:

```console
[Monday, Friday, Wednesday, Tuesday]
MONDAY FRIDAY WEDNESDAY TUESDAY
monday friday wednesday tuesday
```

Another example:

```java
import java.util.*;
import java.io.*;

public class CountKeywords {
  public static void main(String[] args) throws Exception {
    Scanner input = new Scanner(System.in);
    System.out.print("Enter a Java source file: ");
    String filename = input.nextLine();

    File file = new File(filename);
    if (file.exists()) {
      System.out.println("The number of keywords is " + countKeywords(file));
    } else {
      System.out.println("File " + filename + " doesn't exist!");
    }
  }

  public static int countKeywords(File file) throws Exception {
    // Array of all Java keywords + true, false and null
    String[] keywordString = {"abstract", "assert", "boolean",
        "break", "byte", "case", "catch", "char", "class", "const",
        "continue", "default", "do", "double", "else", "enum",
        "extends", "for", "final", "finally", "float", "goto",
        "if", "implements", "import", "instanceof", "int",
        "interface", "long", "native", "new", "package", "private",
        "protected", "public", "return", "short", "static",
        "strictfp", "super", "switch", "synchronized", "this",
        "throw", "throws", "transient", "try", "void", "volatile",
        "while", "true", "false", "null"};

    Set<String> keywordSet = new HashSet<>(Arrays.asList(keywordString));
    int count = 0;
    Scanner input = new Scanner(file);
    while (input.hasNext()) {
      String word = input.next();
      if (keywordSet.contains(word))
        count++;
    }
    return count;
  }
}
```

- `TreeSet`:

Implements the `SortedSet` interface: elements are arranged in a tree. Searching for an element can be performed in logarithmic time `O(log n)`. Member instances must implement the Comparable interface. The container should be able to determine if a is less than or equal to b or if b is greater than a, enabling proper arrangement of elements in the tree.

```java
import java.util.*;

public class TestTreeSet {
    public static void main(String[] args) {
        // Create a hash set
        Set<String> set = new HashSet<>();

        // Add strings to the set
        set.add("London");
        set.add("Paris");
        set.add("New York");
        set.add("San Francisco");
        set.add("Beijing");
        set.add("New York");

        TreeSet<String> treeSet = new TreeSet<>(set);
        System.out.println("Sorted tree set: " + treeSet);

        // Use the methods in SortedSet interface
        System.out.println("first(): " + treeSet.first());
        System.out.println("last(): " + treeSet.last());
        System.out.println("headSet(\"New York\"): " +
                treeSet.headSet("New York"));
        System.out.println("tailSet(\"New York\"): " +
                treeSet.tailSet("New York"));

        // Use the methods in NavigableSet interface
        System.out.println("lower(\"P\"): " + treeSet.lower("P"));
        System.out.println("higher(\"P\"): " + treeSet.higher("P"));
        System.out.println("floor(\"P\"): " + treeSet.floor("P"));
        System.out.println("ceiling(\"P\"): " + treeSet.ceiling("P"));
        System.out.println("pollFirst(): " + treeSet.pollFirst());
        System.out.println("pollLast(): " + treeSet.pollLast());
        System.out.println("New tree set: " + treeSet);
    }
}
```

Output:

```console
Sorted tree set: [Beijing, London, New York, Paris, San Francisco]
first(): Beijing
last(): San Francisco
headSet("New York"): [Beijing, London]
tailSet("New York"): [New York, Paris, San Francisco]
lower("P"): New York
higher("P"): Paris
floor("P"): New York
ceiling("P"): Paris
pollFirst(): Beijing
pollLast(): San Francisco
New tree set: [London, New York, Paris]
```

> see [NavigableSet](https://docs.oracle.com/javase/8/docs/api/java/util/NavigableSet.html)

### Comparing the Collection Concrete Classes

```java
import java.util.*;

public class SetListPerformanceTest {
  static final int N = 5000;

  public static void main(String[] args) {
    // Add numbers 0, 1, 2, ..., N - 1 to the array list
    List<Integer> list = new ArrayList<>();
    for (int i = 0; i < N; i++)
      list.add(i);
    Collections.shuffle(list); // Shuffle the array list

    // Create a hash set, and test its performance
    Collection<Integer> set1 = new HashSet<>(list);
    System.out.println("Member test time for hash set is " +
      getTestTime(set1) + " milliseconds");
    System.out.println("Remove element time for hash set is " +
      getRemoveTime(set1) + " milliseconds");

    // Create a linked hash set, and test its performance
    Collection<Integer> set2 = new LinkedHashSet<>(list);
    System.out.println("Member test time for linked hash set is " +
      getTestTime(set2) + " milliseconds");
    System.out.println("Remove element time for linked hash set is "
      + getRemoveTime(set2) + " milliseconds");

    // Create a tree set, and test its performance
    Collection<Integer> set3 = new TreeSet<>(list);
    System.out.println("Member test time for tree set is " +
      getTestTime(set3) + " milliseconds");
    System.out.println("Remove element time for tree set is " +
      getRemoveTime(set3) + " milliseconds");

    // Create an array list, and test its performance
    Collection<Integer> list1 = new ArrayList<>(list);
    System.out.println("Member test time for array list is " +
      getTestTime(list1) + " milliseconds");
    System.out.println("Remove element time for array list is " +
      getRemoveTime(list1) + " milliseconds");

    // Create a linked list, and test its performance
    Collection<Integer> list2 = new LinkedList<>(list);
    System.out.println("Member test time for linked list is " +
      getTestTime(list2) + " milliseconds");
    System.out.println("Remove element time for linked list is " +
      getRemoveTime(list2) + " milliseconds");
  }

  public static long getTestTime(Collection<Integer> c) {
    long startTime = System.currentTimeMillis();
    // Test if a number is in the collection
    for (int i = 0; i < N; i++)
      c.contains((int)(Math.random() * 2 * N));
    return System.currentTimeMillis() - startTime;
  }

  public static long getRemoveTime(Collection<Integer> c) {
    long startTime = System.currentTimeMillis();
    for (int i = 0; i < N; i++)
      c.remove(i);
    return System.currentTimeMillis() - startTime;
  }
}
```

Output:

```console
Member test time for hash set is 1 milliseconds
Remove element time for hash set is 4 milliseconds
Member test time for linked hash set is 1 milliseconds
Remove element time for linked hash set is 0 milliseconds
Member test time for tree set is 1 milliseconds
Remove element time for tree set is 2 milliseconds
Member test time for array list is 30 milliseconds
Remove element time for array list is 9 milliseconds
Member test time for linked list is 39 milliseconds
Remove element time for linked list is 15 milliseconds
```

### Map

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401301847182.png)

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401301848932.png)

#### Map Concrete Classes

- `HashMap`: efficient for locating a value, inserting a mapping, and deleting a mapping
- `TreeMap`: which implements SortedMap, efficient for traversing keys in a sorted order
- `LinkedHashMap`: extends HashMap with a linked list implementation that supports an ordering of the entries in the map

```java
import java.util.*;

public class TestMap {
    public static void main(String[] args) {
        // Create a HashMap
        Map<String, Integer> hashMap = new HashMap<>();
        hashMap.put("Smith", 30);
        hashMap.put("Anderson", 31);
        hashMap.put("Lewis", 29);
        hashMap.put("Cook", 29);

        System.out.println("Display entries in HashMap");
        System.out.println(hashMap + "\n");

        // Create a TreeMap from the preceding HashMap
        Map<String, Integer> treeMap = new TreeMap<>(hashMap);
        System.out.println("Display entries in ascending order of key");
        System.out.println(treeMap);
    }
}
```

Output:

```console
Display entries in HashMap
{Lewis=29, Smith=30, Cook=29, Anderson=31}

Display entries in ascending order of key
{Anderson=31, Cook=29, Lewis=29, Smith=30}
```

Another example:

```java
import java.util.*;

public class CountOccurrenceOfWords {
    public static void main(String[] args) {
        // Set text in a string
        String text = "Good morning. Have a good class. " +
                "Have a good visit. Have fun!";

        // Create a TreeMap to hold words as key and count as value
        Map<String, Integer> map = new TreeMap<>();

        String[] words = text.split("[\\s+\\p{P}]");
        for (int i = 0; i < words.length; i++) {
            String key = words[i].toLowerCase();

            if (key.length() > 0) {
                if (!map.containsKey(key)) {
                    map.put(key, 1);
                } else {
                    int value = map.get(key);
                    value++;
                    map.put(key, value);
                }
            }
        }

        // Display key and value for each entry
        map.forEach((k, v) -> System.out.println(k + "\t" + v));
    }
}
```

Output:

```console
a	2
class	1
fun	1
good	3
have	3
morning	1
visit	1
```

## Topic 5: Distributed Programming

### Overview

**Aim**

- Introduce the concept of distributed computing and explain one way of building a distributed system in Java

**Contents**

- Definition
- Challenges
- Styles of Middleware
- Java RMI

### What's a distributed system?

"a collection of independent computers that appears to its users as a
single coherent system" - Tanenbaum and van Steen

"one in which hardware or software components located at networked
computers communicate and coordinate their actions only by passing
messages" - Coulouris, Dollimore and Kindberg

"one that stops you getting work done when a machine you’ve never
even heard of crashes" - Lamport

### Why distributed systems?

Because the world is distributed.

- You want to book a hotel in Prague, but you are in Glasgow
- You want to retrieve money from any ATM, but your bank is in London
- An airplane has 1 cockpit, 2 wings, 4 engines, 10k sensors, etc. Similarly railway networks, and other distributed transport systems.

Because **problems** rarely hit two different places at the same time.

- As a company having only one database server is a bad idea
- Having two in the same room is better, but still risky

Because **joining** forces increases performance, availability, etc.

- High Performance Computing, replicated web servers, etc.

**Some Examples**:

Financial trading

- Support execution of trading transactions
- Dissemination and processing of events

Web search

- The web consists of 50+ billion web pages with an average lifetime of ~2 months
- Modern web engines serve 10+ billion queries/month

Massively multiplayer online games

- Online games for example: Fortnite, 'Among Us', support large numbers of users viewing and changing a common world
- Need for low latency coordination to support gameplay

### Ok, it’s important. But is it easy?

A bank asks you to program their new ATM software. Central bank server stores account information. Remote ATM authenticate customers and deliver money

A first version of the program

- ATM: ignoring authentication and security issues

1. Ask customer how much money they want
2. Send message with <customer ID, withdraw, amount> to bank server
3. Wait for bank server answer: <OK> or <refused>
4. If <OK> give money to customer, else display error message

- Central Server:

1. Wait for messages from ATM: <customer ID, withdraw, amount>
2. Check if enough money to withdraw: send <OK>, else send <refused>

### See, easy

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202402131115871.png)

However

- What if the bank server crashes just after 2 and before 3?
- What if the <OK> message gets lost? Takes too long to arrive?
- What if the ATM crashes after 1, but before 4?

### Fallacies of Distributed Computing

Asserted by Peter Deutsch, Sun Microsystems

1. The network is reliable
2. Latency is zero
3. Bandwidth is infinite
4. The network is secure
5. There is one administrator
6. Transport cost is zero
7. The network is homogeneous
8. Topology doesn't change

### Distributed Development

**Challenging**

- Developing good software is difficult
- Developing good distributed software is even harder

To do this you need help

- Hard to build such systems on bare-bones devices
- Strong need for software platforms

**Middleware**

- Provides a high-level programming abstraction
- Hide the complexity associated with distributed systems,including the underlying forms of heterogeneity

### Middleware

- **Client-server** platforms; for example: Distributed Computing Environment (DCE).
- **Remote Procedure Call (RPC)** Middleware; for example: XML-RPC, JSON-RPC, SOAP
- **Distributed object** technology; for example: CORBA, Java RMI
- **Component-based** programming; for example: Fractal, Enterprise Java Beans, OpenCOM
- **Microservice** architecture: Loosely coupled, testable services using CI/CD
- **Other styles**
  - Resource discovery platforms; for example: Jini
  - Group communication services; for example: JGroups
  - Publish-subscribe systems; for example: JMS
  - Distributed file systems, distributed transaction services, distributed document-based systems, agent-based systems, message-oriented Middleware, P2P technologies, etc.

### RMI

**Remote Method Invocation**: an distributed object Middleware solution offering.

- Reference to remote objects and their data and method members
- Pass objects across the network
- Request-Reply protocol and associated semantics
- Location transparency

**Aim**: to make distributed programming as easy as standard Java programming

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202402131117032.png)

**Example**:

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202402131117056.png)
![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202402131118719.png)

#### RPC Middleware / DOM

- Interaction between objects is done through defined interfaces
- Abstraction, where client software doesn't need to know the details of the implementation
- Platform and language independence
- Evolution of software

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202402131118122.png)

### Implementing RMI

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202402131119639.png)

- Proxies, stubs, dispatchers are auto-generated by the IDL compiler
- Client = service consumer; Server = service provider

#### Key components: client side

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202402131120919.png)

**Proxies**

- Masquerades as a local version of the remote interface
- Redirects calls to client stubs

**Client stubs**

- One stub is created per interface method
- Carries out marshalling of a call into a request message sent to remote end
- Marshalling is the process of transforming the memory representation of an object to a data format suitable for storage or transmission
- Also un-marshals returning replies

#### Key components: server side

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202402131120782.png)

**Dispatcher**

- Receives incoming messages and directs them to an appropriate server stub

**Server stubs skeletons**

- Un-marshals message and then invokes appropriate code body
- Also marshals reply values and initiates transmission back to the client

#### Key components: Registry

To implement the key components of a Registry in Remote Method Invocation, you typically follow these steps:

1. Start the RMI Registry on a designated port, default is 1099:

```
start rmiregistry
```

2. Register server objects with the RMI Registry:

```java
LocateRegistry.createRegistry(port); // If the registry is not already running on the specified port
MyRemoteInterface obj = new MyRemoteObject();
Naming.rebind("rmi://<hostname>:<port>/<ServiceName>", obj);
```

3. Lookup the remote object in client programs:

```java
MyRemoteInterface obj = (MyRemoteInterface) Naming.lookup("rmi://<hostname>:<port>/<ServiceName>");
```

The format of the registry names is as follows:

```
rmi://<hostname>:<port>/<ServiceName>
```

Where:

- `<hostname>` is the IP address or host name where the RMI Registry is running.
- `<port>` is the port number on which the RMI Registry is listening, default is 1099.
- `<ServiceName>` is the name under which the remote object is bound in the RMI Registry.

Examples:

- `rmi://localhost/CalculatorService`
- `rmi://194.80.36.30:1099/ChatService`
- `rmi://www.google.com/MapService`

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202402131121202.png)

#### Disadvantages of dom

- **Synchronous interaction**: client blocks while a method is invoked remotely
- **Distributed garbage collection**: efficiency vs bloat
- **Generated structures are static**: can't be changed at runtime
- **Generated structures are heavyweight**: high cost for simple transactions, for example: embedded devices

### RMI how to

**Step 1** Define interfaces

- Extends the Remote interface: identifies interfaces whose methods may be invoked from non-local JVMs
- Each method throws RemoteException: a ‘catch-all’ for communication-related exceptions

```java
import java.rmi.Remote;
import java.rmi.RemoteException;

public interface PasswordGenerator extends Remote {
    // returns a randomly-generated password
    public String genPassword(int ID) throws RemoteException;
    // returns null if the password is incorrect / doesn't exist
    public byte[] downloadSource(int ID, String password) throws RemoteException;
}
```

**Step 2** Implement the interface, or the server

- Extends UnicastRemoteObject: it can be called remotely, therefore, the compiler needs to generate a stub for it
- RemoteException

```java
import java.rmi.Naming;
import java.rmi.RemoteException;
import java.rmi.server.UnicastRemoteObject;

public class PasswordServer extends UnicastRemoteObject implements PasswordGenerator {
    public String genPassword(int ID) throws RemoteException {
        //implementation
        return null;
    }
    public byte[] downloadSource(int ID, String password) throws RemoteException {
        //implementation
        return null;
    }
}
```

**Step 3** Start the server and announce it

- Create an instance of the server class
- Register the instance to the RMI registry under a given name

```java
public static void main(String[] args) {
    try {
        PasswordServer s = new PasswordServer();
        Naming.bind("//stlinux02.dcs.gla.ac.uk/UnsecurePasswordServer", s);
        System.err.println("Password server is ready");
    } catch (Exception e) {
        System.err.println("Server exception: " + e.toString());
        e.printStackTrace();
    }
}
```

**Step 4** Develop the client

- Create a stub by looking up the server
- Use the instance as if it's a local instance of the server class

```java
import java.rmi.Naming;

public class PasswordClient {
    private static String host = "rmi://stlinux02.dcs.gla.ac.uk/";
    private static String serviceName = "UnsecurePasswordServer";

    public static void main(String[] args) {
        try {
            PasswordGenerator stub = (PasswordGenerator) Naming.lookup(host + serviceName);
            String returnWord = stub.genPassword(123456789);
            System.out.println(returnWord);
        } catch (Exception e) {
            System.err.println("Client exception: " + e.toString());
            e.printStackTrace();
        }
    }
}
```

### Topic 5 Lab

This lab focuses on Remote Method Invocation (RMI), allowing students to understand and implement distributed computing concepts. Steps 5a through 5c are the core exercises, with steps 5d and 5e as advanced, optional challenges.

#### Exercise 5a: Developing an RMI client

- **Objective:** Build a client that communicates with an RMI server, which generates a random password. The server's interface is `PasswordGenerator`, and it's located at `//stlinux02.dcs.gla.ac.uk/UnsecurePasswordServer`. Note: Campus or VPN connection is required.
- **Instructions:** Start with the provided `PasswordClient-starter.java`, rename it to `PasswordClient.java`, and complete the sections marked with _TODO_. Refer to the lecture's example for guidance.

**PasswordClient.java**:

```java
import java.rmi.Remote;
import java.rmi.RemoteException;

public interface PasswordGenerator extends Remote {
  // returns a randomly-generated password
  public String genPassword(int ID) throws RemoteException;

  // returns null if the password is incorrect / does not exist
  public byte[] downloadSource(int ID, String password) throws RemoteException;
}
```

**PasswordClient.java**:

```java
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.OutputStream;
import java.rmi.Naming;

public class PasswordClient {
  private static String host =
      "//stlinux02.dcs.gla.ac.uk/"; // "//localhost" "//stlinux02.dcs.gla.ac.uk/"
  private static String serviceName = "UnsecurePasswordServer";

  public static void writeBytesToFile(byte[] bytes, OutputStream out) {
    try {
      out.write(bytes);
    } catch (Exception e) {
      System.err.println("😱 Client exception while writing to file: " + e.toString());
      e.printStackTrace();
    }
  }

  public static void main(String[] args) {
    try {
      PasswordGenerator stub = (PasswordGenerator) Naming.lookup(host + serviceName);
      int myID = 123456789;
      String returnWord = stub.genPassword(myID);
      System.out.println("Password: " + returnWord);

      OutputStream output = null;
      String outputFilename = "sauce.java";
      try {
        output = new FileOutputStream(outputFilename);
      } catch (FileNotFoundException e) {
        System.err.println("😱 Client exception while trying to create file: " + e.toString());
        e.printStackTrace();
      }
      byte[] bytes = stub.downloadSource(myID, returnWord);
      writeBytesToFile(bytes, output);
      System.out.println("Written file @ " + outputFilename + " 😏");
    } catch (Exception e) {
      System.err.println("😱 Client exception: " + e.toString());
      e.printStackTrace();
    }
  }
}
```

#### Exercise 5b: Getting the server source code

There are two ways to obtain the server code:

###### Easy option

- Download the server code from [here](https://yelkhatib.github.io/PasswordServer.java).

###### Challenging option

- Download the server code using the RMI server's `downloadSource` method. Refer to the `PasswordGenerator` interface for parameter and return type details. Use the provided `writeBytesToFile` method to convert the returned array into a file.

#### Exercise 5c: Peeking behind the curtain

- **Objective:** Understand the server's implementation, focusing on how it's defined and its functionalities.
- **Key Points:**
  - The server extends `UnicastRemoteObject` for remote method invocation.
  - Identify methods that can be called remotely and must declare `RemoteException`.
  - Investigate if the server maintains state and the data structures used.
  - Examine the server's main method and compare its feature with your expectations.

```java
import java.io.File;
import java.io.FileInputStream;
import java.rmi.Naming;
import java.rmi.RemoteException;
import java.rmi.server.UnicastRemoteObject;
import java.security.SecureRandom;
import java.util.HashMap;
import java.util.Map;

public class PasswordServer extends UnicastRemoteObject implements PasswordGenerator {
  private static final String host = "//localhost/"; // "//localhost" "//stlinux02.dcs.gla.ac.uk/"
  private static final String serviceName = "UnsecurePasswordServer";
  private static final int randomWordLength = 12;
  private static Map<Integer, String> wordMap = new HashMap<>();
  private static SecureRandom random = new SecureRandom();
  // dictionaries
  private static final String ALPHA_CAPS = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  private static final String ALPHA = "abcdefghijklmnopqrstuvwxyz";
  private static final String NUMERIC = "0123456789";
  private static final String SPECIAL_CHARS = "!@#$%^&*_=+-/";

  PasswordServer() throws RemoteException {}

  private static String generatePasswordFromDictionaries(int ID, int len, String dic) {
    if (wordMap.containsKey(ID)) {
      System.err.println("ID " + ID + " already has a password! 🧐");
      return wordMap.get(ID);
    }
    String word = "";
    for (int i = 0; i < len; i++) {
      int index = random.nextInt(dic.length());
      word += dic.charAt(index);
    }
    wordMap.put(ID, word);
    return word;
  }

  public String genPassword(int ID) throws RemoteException {
    System.err.println(ID + " is asking for a password 😆");
    String password =
        generatePasswordFromDictionaries(
            ID, randomWordLength, ALPHA_CAPS + ALPHA + SPECIAL_CHARS + NUMERIC);
    System.err.println(ID + ", 🤫 the server whispers back " + password);
    return password;
  }

  private static byte[] readFileToBytes(String filename) {
    byte[] bytes = null;
    try {
      File file = new File(filename);
      bytes = new byte[(int) (file.length())];
      (new FileInputStream(file)).read(bytes);
    } catch (Exception e) {
      System.out.println("😱 Server exception while reading the spec file: " + e.toString());
      e.printStackTrace();
    }
    return bytes;
  }

  public byte[] downloadSource(int ID, String password) throws RemoteException {
    System.err.println("Incoming! ID " + ID + " has sent " + password);
    if (wordMap.containsKey(ID)) {
      String passwordStored = wordMap.get(ID);
      System.err.println(
          "🧐 My records show that ID " + ID + " has the password " + passwordStored);
      if (password.equals(passwordStored)) {
        System.err.println("ID's " + ID + " password matches! 🎉  Sending them the source file...");
        byte[] bytes = readFileToBytes("PasswordServer.java");
        if (bytes == null)
          System.err.println(
              "🤔 Something went wrong with reading the source file into an array of bytes!");
        return bytes;
      } else {
        System.err.println("ID's " + ID + " password does not match! 😢");
      }
    }
    return null;
  }

  public static void main(String[] args) {
    try {
      PasswordServer s = new PasswordServer();
      Naming.rebind(host + serviceName, s);
      System.err.println("Password server is ready 💪");

      // byte[] bytes = readFileToBytes("PasswordServer.java");
      // System.out.println(bytes.toString());
    } catch (Exception e) {
      System.err.println("😱 Server exception: " + e.toString());
      e.printStackTrace();
    }
  }
}
```

#### Exercise 5d (Stretch Target): Running the server locally

- **Objective:** Attempt to run the RMI server on your local machine, editing the host variable to `localhost`.
- **Instructions:** Compile and run the `PasswordServer` class after starting the RMI registry. Note potential issues like `java.net.ConnectException`, `java.rmi.NotBoundException`, or `java.rmi.server.ExportException` and their solutions.

#### Exercise 5e (Stretch Target): Running the modified client

- **Objective:** Modify the client to connect to your local server instance.
- **Instructions:** Identify and edit the part of your code specifying the server address to match the local server's interface.

These exercises are designed to deepen understanding of RMI by practicing client-server communication in a distributed environment.

## Topic 6: Concurrency

### What's concurrency?

Concurrency happens when multiple parts of the program running simultaneously.

**why?**

- utilization: Make use of multi-core processors
- performance: Efficient integration with slow devices, for example: disks
- user-friendliness: Responsive applications

### Concurrency in java

Build concurrent Java programs using **_Thread_**. A Java thread is an object. Like all other objects, they have attributes and methods. Unlike other objects, each has its own **_point of execution_**, and runs by itself.

A thread is an object whose code runs in parallel with the rest of the program.

There are two ways to create a thread:

- extend the Thread class and implement the `run` method

```java
private static class PrinterThread extends Thread {
    private String message;
    private int n;

    public PrinterThread(String message, int n) {
        this.message = message;
        this.n = n;
    }

    public void run() {
        for (int i = 0; i < n; i++) {
            System.out.println(i + "/" + n + " " + message);
        }
    }
}

public static void main(String[] args) {
    PrinterThread p = new PrinterThread("Hello", 10);
    p.start();
}
```

> It's the easiest approach, but it's not recommended because it doesn't allow you to extend any other class.

- implement the `Runnable` interface, then create a `Thread` object passing an instance of your class.

Either way, you must override the `run` method.

To start the thread, call `Thread.start()`, **not** `run ()`.

**Implementing Runnable**

```java
public class PrinterRunnable implements Runnable {
    private String message;
    private int n;

    public PrinterRunnable(String message, int n) {
        this.message = message;
        this.n = n;
    }

    public void run() {
        for(int i = 0; i < n; i++) {
            System.out.println(i + "/" + n + " " + message);
        }
    }
}
```

To use this class, create an instance and then **pass it to an instance of the `Thread` class**.

```java
public class RunnableSingleThread {
    public static void main(String[] args) {
        PrinterRunnable p = new PrinterRunnable("Hello", 10);
        Thread t = new Thread(p);
        t.start(); // starts the thread by invoking the run() method of PrinterThread
    }
}
```

### Creating multiple threads

Create many Threads via an **array** of Thread objects.

```java
public class RunnableMultipleThreads {
    public static void main(String[] args) {
        int numThreads = 2;
        Thread[] threads = new Thread[numThreads];

        for (int i = 0; i < numThreads; i++) {
            threads[i] = new Thread(new PrinterRunnable("I'm thread " + i, 5));
            threads[i].start();
        }
    }
}
```

Output:

```console
0/5 I'm thread 0
0/5 I'm thread 1
1/5 I'm thread 0
1/5 I'm thread 1
2/5 I'm thread 0
2/5 I'm thread 1
3/5 I'm thread 0
3/5 I'm thread 1
4/5 I'm thread 0
4/5 I'm thread 1
```

Both Threads are running at the same time. There are \*_no guarantees_ about the order of their execution, hence the need for concurrent programming 'good practices'.

### Mapping to hardware

CPU has many cores, 4–16 typical, each can run one thread at a time. If fewer threads than cores, all threads truly run in parallel. If more threads than cores, Java / OS does time-slicing, which means, run the first thread for a while, then the second, then the first, etc. It's hard to predict when switches occur.

### Thread priorities

Each Thread has a priority which can be read using `getPriority()`

- MIN PRIORITY = 1
- NORM PRIORITY = 5, default
- MAX PRIORITY = 10

Priority can be changed using `setPriority(int)`

- value between 1 and 10

Threads inherit priority of their creator, if not specified.

### Thread control

![](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202402131257683.png)

### Thread Interruption

Blocking methods are those that rely on something outside themselves for termination

- waiting for a timer to elapse, another Thread to end, etc.

Waiting on other systems might take forever, or at least the user might get bored, hence, Threads can be interrupted by other Threads calling
`someThread.interrupt()`

Two possible outcomes:

- if it's running an `interruptable` method, for example sleep, the method unblocks and throws the `InterruptedException`
- otherwise, its interrupted status is set to TRUE.

Methods for reading interrupted status:

- `isInterrupted()`: read only
- `interrupted()`: read and clear

### Thread control

`sleep(long milliseconds)` puts the Thread to sleep.

```java
Thread.sleep(1000); // sleep for 1 second
```

> `sleep()` may throw an `InterruptedException` if `interrupt()` is called.

`yield()` temporarily releases time for other Threads, if any.

```java
Thread.yield();
```

`join()` pauses the current Thread till another one ends. Here `someThread.join()` pauses `main` till `someThread` ends.

```java
public static void main(String[] args) {
    MyThread t = new MyThread();
    t.start();
    t.join();
    System.out.print("thread t completed");
}
```

Wait until all five threads have finished.

```java
MyThread[] m = new MyThread[5];

for(int i = 0; i < 5; i++) {
    m[i] = new MyThread();
    m[i].start();
}

for(int i = 0; i < 5; i++) {
    m[i].join();
}

System.out.print("all threads completed");
```

This doesn't work. Why?

```java
MyThread[] m = new MyThread[5];

for (int i = 0; i < 5; i++) {
    m[i] = new MyThread();
    m[i].start();
    m[i].join();
}

System.out.print("all threads completed");
```

The code provided won't work as intended because calling `m[i].join()` immediately after `m[i].start()` within the same loop cause the main thread to wait for each thread to complete before proceeding to start the next thread. This defeats the purpose of multi-threading, where you typically want threads to run concurrently rather than sequentially. As a result, your program behave as if you are running the threads sequentially, rather than concurrently.

To allow all threads to run concurrently and then wait for all of them to complete before printing "all threads completed," you should call `join()` after starting all the threads in the second loop.

### Thread names

Threads can be optionally given names through their constructor

```java
Thread t = new Thread(aRunnableThing,"cool name");
Thread t = new Thread("cool name");
```

Use `setName(String)` and `getName()`, within a class that extends Runnable, you have to obtain the relevant Thread object first:

```java
Thread.currentThread().getName()
```

### Benefits of multi-threading

Sorting the values in a large array can be made parallel:

1. split the array into N smaller arrays
2. sort the smaller arrays
3. merge the results together

```java
public class ThreadSort implements Runnable {
	private Double[] subArray;
	private int arrayLength;
	public ThreadSort(Double[] subArray) {
		arrayLength = subArray.length;
		this.subArray = new Double[arrayLength];
		for(int i=0;i<arrayLength;i++) {
			this.subArray[i] = subArray[i];
		}
	}
	public void run() {
		// Do a slow bubble sort
		int n_changes = 1;
		while(n_changes > 0) {
			n_changes = 0;
			for(int i=0;i<arrayLength-1;i++) {
				if(subArray[i+1] < subArray[i]) {
					Double temp = subArray[i];
					subArray[i] = subArray[i+1];
					subArray[i+1] = temp;
					n_changes += 1;
				}
			}
		}
	}
	public Double[] getArray() {
		return subArray;
	}
}

public class MergeSort {
  public static void main(String[] args) {
    // Make some random values
    int n_values = 1000;
    Double[] values = new Double[n_values];
    for (int i = 0; i < n_values; i++) {
      values[i] = Math.random();
    }

    int n_threads = 10;
    int n_vals_per_thread = n_values / n_threads;
    ThreadSort[] sorters = new ThreadSort[n_threads];
    Thread[] threads = new Thread[n_threads];

    // Copy subsets of the array.
    // Note we could have used Arrays.copy instead of the loop
    int pos = 0;
    for (int i = 0; i < n_threads; i++) {
      Double[] temp = new Double[n_vals_per_thread];
      for (int j = 0; j < n_vals_per_thread; j++) {
        temp[j] = values[j + pos];
      }
      pos += n_vals_per_thread;
      // Make a new ThreadSort object (it implements Runnable)
      sorters[i] = new ThreadSort(temp);
      // Put it in a thread
      threads[i] = new Thread(sorters[i]);
      // Start the thread
      threads[i].start();
    }

    // Collect all of the results
    Double[][] subArrays = new Double[n_threads][n_vals_per_thread];
    for (int i = 0; i < n_threads; i++) {
      try {
        threads[i].join();
      } catch (InterruptedException e) {
      }
      subArrays[i] = sorters[i].getArray();
    }

    // Do the merge
    // pointers holds where we are in each list
    int[] pointers = new int[n_threads];
    Double[] sorted = new Double[n_values];
    for (int i = 0; i < n_values; i++) {
      // find the lowest
      Double lowest = 1.0;
      int lowPos = -1;
      for (int j = 0; j < n_threads; j++) {
        if (pointers[j] < n_vals_per_thread) {
          if (subArrays[j][pointers[j]] < lowest) {
            lowest = subArrays[j][pointers[j]];
            lowPos = j;
          }
        }
      }
      pointers[lowPos] += 1;
      sorted[i] = lowest;
    }
    for (int i = 0; i < n_values; i++) {
      System.out.println(sorted[i]);
    }
  }
}
```

Output:

```console
.0012076865998164044
0.0012235151763752006
0.0025076197028002234
0.003598420406430325
0.005685167260666257
0.0075192342487062636
0.009427614994794387
0.0098650299919113
0.010721266034570909
0.012219669675103018
0.014512260733102522
0.015127019135716124
0.016290676527065062
0.017156458884038717
0.01889342074445166
...
0.9932076758140784
0.9949651919516266
0.9956700125477551
0.9958731298460722
0.9971765613680127
0.9984018191029964
0.9995128056561186
```

How much speed-up can this give?

### Data parallelism

All threads ‘see’ the same Java objects in memory. Given a large array, each thread can perform a calculation on some part of it. Write the results to disjoint regions of a pre-allocated output array. It's efficient since it makes good use of multi-core CPU.

```java
public class ParallelMath {
  static class RunOnSubarray implements Runnable {
    double[] inputArray, outputArray;
    int firstIndex, count;

    public RunOnSubarray(double[] inputArray, double[] outputArray, int firstIndex, int count) {
      this.inputArray = inputArray;
      this.outputArray = outputArray;
      this.firstIndex = firstIndex;
      this.count = count;
    }

    public void run() {
      for (int index = firstIndex; index < firstIndex + count; ++index) {
        final double input = inputArray[index];
        outputArray[index] = calculateExpSlowly(input);
      }
    }

    static double calculateExpSlowly(final double x) {
      // This calculates exp(x) using a Taylor series. The maths is not important. What matters is
      // that
      // it's a slooow calculation. In real life you would use Math.exp(x)
      double result = 1.;
      double factorial = 1.;
      for (int n = 1; n < 1000; ++n) {
        factorial *= n;
        result += Math.pow(x, n) / factorial;
      }
      return result;
    }
  }

  public static void main(String[] args) throws InterruptedException {
    final int numInputs = 1000000;
    double[] inputs = new double[numInputs];
    double[] outputs = new double[numInputs];
    for (int index = 0; index < numInputs; ++index) {
      inputs[index] = Math.random();
    }
    final int numThreads = 4;
    final int countPerThread = numInputs / numThreads;
    assert numInputs % numThreads
        == 0; // the number of threads must exactly divide the number of inputs
    final Thread[] threads = new Thread[numThreads];
    for (int threadIndex = 0; threadIndex < numThreads; ++threadIndex) {
      final int firstIndex = threadIndex * countPerThread;
      threads[threadIndex] =
          new Thread(new RunOnSubarray(inputs, outputs, firstIndex, countPerThread));
      threads[threadIndex].start();
    }
    for (int threadIndex = 0; threadIndex < numThreads; ++threadIndex) {
      threads[threadIndex].join();
    }
  }
}
```

### Executors

Thread creation is relatively expensive. It unnecessarily ties together concepts of creating threads and running tasks on them. It's difficult to control how many threads running in a complex app.

Executors are a higher-level interface for creating threads. Executor object manages thread creation. Runnable tasks are submitted to the executor to run on some thread.

```java
public interface Executor {
  void execute(Runnable command);
}
```

#### Tread pools

Simplest Executor implementation: fixed thread pool.

- create with `ExecutorService.newFixedThreadPool(nThreads)`

This starts a pool containing a maximum of `nThreads` threads.

```java
ExecutorService pool = Executors.newFixedThreadPool(8);
Runnable r1 = new MyRunnable(); // runnable that performs some task
Runnable r2 = new OtherRunnable();
pool.execute(r1);
pool.execute(r2);
```

`ExecutorService.shutdown` is similar to `Thread.join`, wait for all tasks to finish, then stop all the threads.

`ExecutorService.awaitTermination` waits for shutdown to complete

`ExecutorService.shutdownNow` interrupts all threads

### Additional reading

[https://www.geeksforgeeks.org/multithreading-in-java/](https://www.geeksforgeeks.org/multithreading-in-java/)

[https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html](https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html)

### Topic 6 Lab

#### 1. Creating Threads

##### Exercise 1

- **Objective:** Create a class implementing the `Runnable` interface. In its `run()` method, it should sleep for a random number of seconds before printing a message to the console. Test this by starting a `Thread` in a main method.
- **Note:** Handle the `InterruptedException` from `Thread.sleep()`.

```java
class MyRunnable implements Runnable {
  public void run() {
    int numSeconds = new Random().nextInt(10) + 1;
    try {
      Thread.sleep(numSeconds * 1000);
    } catch (InterruptedException e) {
      return;
    }
    System.out.println("Slept for " + numSeconds + " seconds");
  }
}

public class CreatingThreads1 {
  public static void main(String[] args) {
    MyRunnable myRunnable = new MyRunnable();
    Thread myThread = new Thread(myRunnable);
    myThread.start();
  }
}
```

##### Exercise 2

- **Objective:** Similar to the previous exercise, but extend the `Thread` class instead of implementing `Runnable`.

```java
class MyThread extends Thread {
  public void run() {
    int numSeconds = new Random().nextInt(10) + 1;
    try {
      Thread.sleep(numSeconds * 1000);
    } catch (InterruptedException e) {
      return;
    }
    System.out.println("Slept for " + numSeconds + " seconds");
  }
}

public class CreatingThreads1 {
  public static void main(String[] args) {
    MyThread myOtherThread = new MyThread();
    myOtherThread.start();
  }
}
```

##### Exercise 3

- **Objective:** Utilize the `Runnable` class from the first exercise to create and start multiple threads in a new main method.

```java
public class CreatingThreads2 {
    public static void main(String[] args) {
        int numThreads = 5;
        Thread[] threads = new Thread[numThreads];
        for (int i = 0; i < numThreads; i++) {
            MyRunnable myRunnable = new MyRunnable();
            threads[i] = new Thread(myRunnable);
            threads[i].start();
        }
    }
}
```

##### Exercise 4

- **Objective:** Modify Exercise 3 by assigning names to each `Thread` and include the thread name in the completion message.

```java
            threads[i] = new Thread(myRunnable, "Thread #" + i);
```

##### Exercise 5

- **Objective:** Ensure the main method prints a message immediately after starting all threads, which should display before the threads complete.

```java
public class CreatingThreads2 {
  public static void main(String[] args) {
    int numThreads = 5;
    Thread[] threads = new Thread[numThreads];
    for (int i = 0; i < numThreads; i++) {
      MyRunnable myRunnable = new MyRunnable();
      threads[i] = new Thread(myRunnable, "Thread #" + i);
      threads[i].start();
    }
    System.out.println("All threads started!");
  }
}
```

##### Exercise 6

- **Objective:** Implement `Thread.join()` to make the main thread wait for all others to complete before it prints its message.

```java
public class CreatingThreads2 {
  public static void main(String[] args) {
    int numThreads = 5;
    Thread[] threads = new Thread[numThreads];
    for (int i = 0; i < numThreads; i++) {
      MyRunnable myRunnable = new MyRunnable();
      threads[i] = new Thread(myRunnable, "Thread #" + i);
      threads[i].start();
    }
    for (int i = 0; i < numThreads; i++) {
      try {
        threads[i].join();
      } catch (InterruptedException e) {
      }
    }
    System.out.println("All threads finished!");
  }
}
```

#### 2. Max Finder

**Setup:** use the following code snippet to generate a 2D array of random `Double` values between 0 and 1 as test inputs:

```java
int nRows = 100;
int nCols = 50;
Double[][] randArray = new Double[nRows][nCols];
for(int r=0; r<nRows; r++) {
    for(int c=0; c<nCols; c++) {
        randArray[r][c] = Math.random();
    }
}
```

1. Within a main method, employ loops to find the maximum value in `randArray`. This serves to test your threaded solution.

2. Create a `Runnable` implementation that finds the max of a 1D array. The constructor should accept the 1D array, a result storage array, and the result position. Use a separate instance for each row in `randArray`, storing each maximum in a single result array. Start and join all threads, leaving a 1D array of maximum values.

3. Pass the array from Exercise 2 to a final `Runnable` instance to find the overall maximum. Start and join this thread, then print the final maximum value, which should match the initial non-threaded result.

```java
public class MaxFinder {

    // Runnable class to find max of a 1D array (Q2.2)
    public static class MaxRow implements Runnable {
        Double[] row;
        int rowNumber;
        Double[] rowMaxes;

        public MaxRow(Double[] row, Double[] rowMaxes, int rowNumber) {
            this.row = row;
            this.rowMaxes = rowMaxes;
            this.rowNumber = rowNumber;
        }

        public void run() {
            rowMaxes[rowNumber] = 0.0;
            for (int i = 0; i < row.length; i++) {
                if (row[i] > rowMaxes[rowNumber]) {
                    rowMaxes[rowNumber] = row[i];
                }
            }
        }
    }

    public static void main(String[] args) {
        int nRows = 100;
        int nCols = 50;
        Double[][] randArray = new Double[nRows][nCols];
        for (int r = 0; r < nRows; r++) {
            for (int c = 0; c < nCols; c++) {
                randArray[r][c] = Math.random();
            }
        }

        // Find the max using loops (Q2.1)
        Double max = 0.0;
        for (int r = 0; r < nRows; r++) {
            for (int c = 0; c < nCols; c++) {
                if (randArray[r][c] > max) {
                    max = randArray[r][c];
                }
            }
        }
        System.out.println(max);

        // Threaded solution (Q2.3)
        Double[] rowMaxes = new Double[nRows];
        Thread[] threads = new Thread[nRows];
        for (int i = 0; i < nRows; i++) {
            threads[i] = new Thread(new MaxRow(randArray[i], rowMaxes, i));
            threads[i].start();
        }
        try {
            for (int i = 0; i < nRows; i++) {
                threads[i].join();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        Double[] finalVal = new Double[1];
        Thread finalThread = new Thread(new MaxRow(rowMaxes, finalVal, 0));
        finalThread.start();
        try {
            finalThread.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(finalVal[0]);
    }
}
```

#### 3. Data Parallelism and Thread Pools

1. Modify the `ParallelMath` example to handle cases where the number of inputs isn't exactly divisible by the number of threads.

   ```java
     public static void main(String[] args) throws InterruptedException {
         final int numInputs = 1000003;  // not divisible by numThreads (Q3.1)
         double[] inputs = new double[numInputs];
         double[] outputs = new double[numInputs];
         for (int index = 0; index < numInputs; ++index) {
             inputs[index] = Math.random();
         }
         final int numThreads = 4;
         final int countPerThread = numInputs / numThreads + 1;  // allow for one extra calculation per thread to handle 'leftovers' (Q3.1)
         final Thread[] threads = new Thread[numThreads];
         for (int threadIndex = 0; threadIndex < numThreads; ++threadIndex) {
             final int firstIndex = threadIndex * countPerThread;
             final int countForThread = Math.min(countPerThread, numInputs - firstIndex);  // make sure the thread doesn't go beyond the last value (Q3.1)
             threads[threadIndex] = new Thread(new RunOnSubarray(inputs, outputs, firstIndex, countForThread));
             threads[threadIndex].start();
         }
         for (int threadIndex = 0; threadIndex < numThreads; ++threadIndex) {
             threads[threadIndex].join();
         }
     }
   ```

2. Ensure that both the original and modified implementations produce the same results.

3. Compare the performance of the original and revised implementations to see if there's a performance trade-off for increased flexibility.

4. Alter the implementation to utilize a `FixedThreadPool` for thread management instead of manually creating threads.
   ```java
       public static void main(String[] args) throws InterruptedException {
       final int numInputs = 1000003;  // not divisible by numThreads (Q3.1)
       double[] inputs = new double[numInputs];
       double[] outputs = new double[numInputs];
       for (int index = 0; index < numInputs; ++index) {
           inputs[index] = Math.random();
       }
       final int numThreads = 4;
       final int countPerThread = numInputs / numThreads + 1;  // allow for one extra calculation per thread to handle 'leftovers' (Q3.1)
       ExecutorService pool = Executors.newFixedThreadPool(numThreads);  // create a thread-pool (Q3.4)
       for (int threadIndex = 0; threadIndex < numThreads; ++threadIndex) {
           final int firstIndex = threadIndex * countPerThread;
           final int countForThread = Math.min(countPerThread, numInputs - firstIndex);  // make sure the thread doesn't go beyond the last value (Q3.1)
           pool.execute(new ParallelMath.RunOnSubarray(inputs, outputs, firstIndex, countForThread));
       }
       pool.shutdown();
       pool.awaitTermination(60, TimeUnit.SECONDS);  // wait for all threads to complete (Q3.4)
   }
   ```

## Topic 7: Syncronization

### Race conditions

A benefit of threads is the shared address space. Multiple threads can access the same Java object. It's efficient because it avoids copying data between threads. However, if many threads access the same shared object, it can lead to race conditions and data corruption.

For example, if several threads increment the same counter, like counting number of visitors to a website. If there are 100 threads all incrementing the same counter 1000 times then the result should be 100,000. But it's not guaranteed.

```java
public void run(){
  for(int i= 0; i< n; i++){
    int temp= count.getCount();
    temp++;
    count.setCount(temp);
  }
}
```

Remember, we've no idea when Java will move from one thread to another, it could switch thread between the `getCount()` and `setCount()` method calls.

**with one thread:**
![202404151158615.png](https://raw.githubusercontent.com/jkmeansesc/picwen/main/img/202404151158615.png)

**with two threads (one possible scenario):**
![202404151159004.png](https://raw.githubusercontent.com/jkmeansesc/picwen/main/img/202404151159004.png)

If control is passed between `get` and `set`, the new thread sees an out-of-date value, remember that `temp` is local to each thread. In fact, thread 1 might be sitting dormant with out-of-date variable values for a long time. When it finally runs and updates `temp`, it will effectively overwrite lots of updates performed by other threads.

This is a **_race condition_**: occurs if the effect of multiple threads on shared data depends on the order in which they're scheduled.

This is a big problem in multi-threaded programs.

### Non-Atomicity

Non-atomicity refers to a property of an operation or a series of operations that cannot be executed as a single, indivisible unit. In other words, a non-atomic action can be interrupted or interleaved with other actions, potentially resulting in incomplete, inconsistent, or unintended outcomes.

`count++` really means:

1. retrieve count from RAM to the CPU
2. add one
3. write back to RAM

Hence still have a problem if switching threads.
![202404151235216.png](https://raw.githubusercontent.com/jkmeansesc/picwen/main/img/202404151235216.png)

### Memory Caching

![202404151237874.png](https://raw.githubusercontent.com/jkmeansesc/picwen/main/img/202404151237874.png)

All variables and objects are stored in main memory. Several layers of cache store copies for faster read and write operations. When a CPU modifies a variable, it updates its cache first. Changes are only visible to other threads in main memory later.

### Synchronized Methods

To avoid **_race conditions_**, we **_synchronize_** threads by restricting which parts of code can be run simultaneously.

```java
public static class MyCounter{
  private int count= 0;
  public synchronized void increment(){
    count++;
  }
  public synchronized void decrement(){
    count--;
  }
  public synchronized int getCount(){
    return count;
  }
}
```

Only one thread can enter `increment` at any given time, other threads are **blocked** if they try. And only one thread can enter **any** synchronized method of **that object** at any given time. This means that when one thread is executing any of the synchronized methods (`increment`, `decrement`, or `getCount`) of a particular `MyCounter` object, all other threads are prevented from simultaneously accessing any of the object's synchronized methods, ensuring that the shared `count` variable remains exclusively accessible to the currently executing thread until it completes its operation.

### Monitors

Monitor, or monitor lock, is the hidden object associated with each object in the program. It's used to control access to the object's synchronized methods. When a thread enters a synchronized method, it acquires the monitor lock, and when it exits the method, it releases the lock. Only one thread can own each object's monitor at any moment.

`synchronized` methods acquire monitor lock of `this`.

```java
public static class MyCounter {
  private int count = 0;

  public synchronized void increment() {
      count++;
    }
  }
  // ... other methods ...
}
```

… is equivalent to

```java
public static class MyCounter {
  private int count = 0;

  public void increment() {
    synchronized (this) {
      count++;
    }
  }
  // ... other methods ...
}
```

The `AnimalCounter` class serves as a practical demonstration of how to employ different synchronization locks (using the synchronized keyword with distinct objects) to achieve fine-grained control over concurrent access to specific shared resources within a single class. The example keeps track of the counts of sheep and cows, each represented by their respective counters (`sheepCounter` and `cowCounter`) and associated lists (sheep and cow ArrayLists). By synchronizing on different objects—specifically, the sheep and cow ArrayLists—when manipulating the animal counts or retrieving their current values, the class ensures that updates to one type of animal counter do not interfere with operations on the other, effectively isolating concurrent access and protecting against race conditions.

```java
public static class AnimalCounter {
  private int sheepCounter = 0;
  private int cowCounter = 0;
  private ArrayList<Sheep> sheep;
  private ArrayList<Cow> cows;

  public void newSheepAndCow() {
    synchronized (sheep) {
      sheepCounter++;
      // append new instance to sheep list
    }
    synchronized (cows) {
      cowCounter++;
      // append new instance to cow list
    }
  }

  public int getSheepCount() {
    synchronized (sheep) {
      return sheepCounter;
    }
  }

  public int getCowCount() {
    synchronized (cows) {
      return cowCounter;
    }
  }
}
```

Where to place `synchronized`? On what object?

**where**: only where it is _needed_ to prevent race conditions because you are asking Java to slow down at a certain point.

**what**: the object you want only one thread to access and manipulate at a time.

### Locks

A lock is an object used to control the threads that want to manipulate a shared resource.

```java
public static class MyCounter {
    private int count = 0;
    private ReentrantLock counterLock = new ReentrantLock();

    public void increment() {
        counterLock.lock();
        count++;
        counterLock.unlock();
    }

    // ...
}
```

When `counterLock` is locked, no other thread can lock it until it has been release.

> If the code between `lock()` and `unlock()` throws an exception, then the lock is never released.

Therefore, always do the following to ensure the lock is released:

```java
myLock.lock();
try {
    // some code
} finally {
    myLock.unlock();
}
```

### Deadlocks

What if two threads are each waiting for the other to release a lock? The program will hang indefinitely. This is a **_deadlock_**.

```java
import java.util.concurrent.locks.*;

public class CounterDecounter {
    public static class MyCounter {
        // MyCounter now has increment and decrement methods each with locks
        // Now the locks are also in try blocks
        private int count = 0;
        private ReentrantLock counterLock = new ReentrantLock();

        public void increment(int amount) {
            counterLock.lock();
            try {
                count += amount;
                System.out.println("Adding " + amount + ", result " + count);
            } finally {
                counterLock.unlock();
            }
        }

        public void decrement(int amount) {
            counterLock.lock();
            try {
                count -= amount;
                System.out.println("Subtracting " + amount + ", result " + count);
            } finally {
                counterLock.unlock();
            }
        }

        public int getCount() {
            return count;
        }
    }

    public static class Counter extends Thread {
        private MyCounter count;
        private int n;
        private int amount;

        public Counter(MyCounter count, int n, int amount) {
            this.count = count;
            this.n = n;
            this.amount = amount;
        }

        public void run() {
            for (int i = 0; i < n; i++) {
                count.increment(amount);
            }
        }
    }

    public static class DeCounter extends Thread {
        private MyCounter count;
        private int n;
        private int amount;

        public DeCounter(MyCounter count, int n, int amount) {
            this.count = count;
            this.n = n;
            this.amount = amount;
        }

        public void run() {
            for (int i = 0; i < n; i++) {
                count.decrement(amount);
            }
        }
    }

    public static void main(String[] args) {
        MyCounter count = new MyCounter();
        // Create some counters and decounters
        // such that we will end up with zero
        int nCounters = 10, incNum = 10;
        int nDeCounters = 2, decNum = 50;
        int nReps = 10;

        Counter[] c = new Counter[nCounters];
        DeCounter[] d = new DeCounter[nDeCounters];

        // Create and start the counters and decounters
        for (int i = 0; i < nDeCounters; i++) {
            d[i] = new DeCounter(count, nReps, decNum);
            d[i].start();
        }
        for (int i = 0; i < nCounters; i++) {
            c[i] = new Counter(count, nReps, incNum);
            c[i].start();
        }

        // Wait for everything to finish
        try {
            for (int i = 0; i < nCounters; i++) {
                c[i].join();
            }
            for (int i = 0; i < nDeCounters; i++) {
                d[i].join();
            }
        } catch (InterruptedException e) {
            // Do nothing
        }
        System.out.println("Final Result: " + count.getCount());
    }
}
```

In this example, we want to ensure counter never goes negative, we could put some kind of wait condition in the `decrement` method:

```java
        public void decrement(int amount) {
            // This decrement method sleeps whilst it waits for the total to increase
            // enough to make a decrement not leave the total negative
            // Unfortunately, due to the lock, it can cause a deadlock.
            counterLock.lock();
            try {
                while (count < amount) {
                    Thread.sleep(1);
                }
                count -= amount;
                System.out.println("Subtracting " + amount + ", result " + count);
            } catch (InterruptedException e) {
                // Fall through
            } finally {
                counterLock.unlock();
            }
        }
```

This causes the program to hang whenever it tries to decrement by an amount that is greater than `count` because the thread has locked `counterLock`, no other thread can increase the amount.

When a `decrement` thread tries to subtract, it waits for the count to be big enough before it does the subtraction. But because it's waiting, it's holding onto `conterLock` which means other threads can't `increment` to make the `amount` big enough, hence the deadlock.

### Conditions

Conditions let threads temporarily unlock locks whilst they await some condition to be fulfilled. In this case, we'd like to temporarily unlock within a thread that is waiting to decrement.

```java
import java.util.concurrent.locks.*;

public class CounterDecounter3 {
    public static class MyCounter {
        private int count = 0;
        private ReentrantLock counterLock = new ReentrantLock();
        // We now create a condition from our lock
        private Condition bigEnough = counterLock.newCondition();

        public void increment(int amount) {
            counterLock.lock();
            try {
                count += amount;
                System.out.println("Adding " + amount + ", result " + count);
                // Every time an amount is added, signalAll is invoked to wake up any
                // threads that have called await()
                bigEnough.signalAll();
            } finally {
                counterLock.unlock();
            }
        }

        public void decrement(int amount) {
            counterLock.lock();
            try {
                while (count < amount) {
                    // when await is called, the current thread pauses until
                    // another thread calls the signalAll() method of the condition
                    bigEnough.await();
                    // Once signalAll has been received, if count>amount the decrement
                    // will happen, otherwise it will await() again
                }
                count -= amount;
                System.out.println("Subtracting " + amount + ", result " + count);
            } catch (InterruptedException e) {
                // Fall through
            } finally {
                counterLock.unlock();
            }
        }

        public int getCount() {
            return count;
        }
    }

    public static class Counter extends Thread {
    // ...
    }

    public static class DeCounter extends Thread {
    // ...
    }

    public static void main(String[] args) {
    // ...
    }
}
```

A thread calling `decrement` when `count < amount` will wait until another thread invokes the `Condition.signalAll()` method. Whenever an increment is made, all threads waiting on this condition are restarted. Note that the `signalAll()` doesn't mean that the amount is big enough, the syntax in `decrement()` means that `signallAll()` will cause the thread to check again, it might just ends up invoking `await()` again.

### The Thread Lifecycle

![202404161945294.png](https://raw.githubusercontent.com/jkmeansesc/picwen/main/img/202404161945294.png)

A thread is always in one of 6 states

- trying to acquire a lock makes a thread "blocked"
- `join()` and `await()` put a thread in "waiting"
- `sleep()` puts a thread in "timed waiting"

### Immutability

Avoid mutable state, specifically, avoid mutable variables shared between threads. Use `final` to make variables immutable.

Modify objects only from one thread. Read only after all threads completed.

### Atomic Variables

The term "atomic" refers to the property that these variables' operations are guaranteed to execute entirely or not at all, without the possibility of being interrupted by other threads during execution. This ensures data integrity and eliminates the need for explicit synchronization mechanisms like locks or semaphores for individual variable updates.

> see [java.concurrent.atomic](https://docs.oracle.com/javase/8/docs/api/index.html?java/util/concurrent/atomic/package-summary.html)

1. **Indivisibility**: Atomic operations are indivisible, meaning they appear to execute instantaneously from the perspective of other threads. No thread can observe a partially updated state during an atomic operation, as the entire operation is treated as a single, uninterruptible unit of work.
2. **Thread-Safe Operations**: Atomic variables replace several primitive types (such as int, long, boolean, etc.) and provide thread-safe alternatives that offer operations like read, write, increment, decrement, compare-and-swap, and exchange, which are inherently safe for use in concurrent scenarios.
3. **Package `java.concurrent.atomic`**: Java's java.concurrent.atomic package contains classes like `AtomicInteger`, `AtomicLong`, `AtomicBoolean`, and others, which encapsulate the capability of atomic variables. For instance, the `AtomicInteger` class offers methods like `get()` to retrieve the current value, `getAndAdd(int delta)` to atomically add a given value to the current value, and `getAndIncrement()` to increment the current value atomically while returning the value before the increment.

### Volatile Variables

`valatile` keyword can be applied to variable declarations

```java
priavte volatile int myInt;
```

Any write to a volatile variable is immediately visible to all threads because it bypass cache and writes directly into main memory. It's a way to ensure that all threads see the same value of a variable.

### Concurrent Collections

Collections are useful, such as `ArrayList<T>`, `HashMap<K,V>` etc, but they are not thread-safe. For example, `ArrayList.add` writes at current 'cursor' position, then increments it. It is not atomic. Two concurrent calls to add yield race conditions: values collide.

`java.util.concurrent` provides concurrent collections. It's safe to read/write from multiple threads, often faster than `synchronized` and `ReentrantLock`. Check-then-act race conditions still can occur:

```java
ConcurrentHashMap<String, Integer> m = new ConcurrentHashMap<String, Integer>();

void addIfMissing(String key, int value) {
    if (!m.containsKey(key)) {
        m.put(key, value);
    }
}
```

To illustrate this race condition, consider the following scenario:

1. Two threads, Thread X and Thread Y, both attempt to call `addIfMissing` with the same key, say `uniqueKey`, and different integer values.
2. Both threads concurrently reach the if statement and independently check whether `uniqueKey` is in the map.
3. Since neither thread has yet inserted the key, both threads find it missing. Without proper synchronization, it is possible that both threads proceed to insert the key-value pair, even though the intention was to add it only once.

To resolve this race condition, you would typically employ a technique that ensures the atomicity of the check and the insert operation combined, such as using the `putIfAbsent` method provided by `ConcurrentHashMap`, which performs the check and insert in a single, atomic step:

```java
void addIfMissing(String key, int value){
    m.putIfAbsent(key, value);
}
```

### Executors

Thread creation is relatively expensive, it unnecessarily ties together concepts of _creating_ threads and _running tasks_ on them. It is difficult to control how many threads running in a complex app.

Executors are a higher-level interface for creating threads. `runnable` tasks are submitted to the executor to run on some thread.

### Thread Pools

The simplest executor implementation is a fixed thread pool, created using `ExecutorService.newFixedThreadPool(nThreads)`, where `nThreads` specifies the maximum number of threads in the pool.

Create with `ExecutorService.newFixedThreadPool(nThreads)`, this will start a pool containing a maximum of `nThreads` threads.

```java
public class ExecutorDemo {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(2);

        // Submitting Runnable tasks to the executor
        executor.execute(new MyRunnable());
        executor.execute(new OtherRunnable());

        // Initiating graceful shutdown of the executor
        executor.shutdown();

        // Waiting for the executor to terminate
        try {
            executor.awaitTermination(Long.MAX_VALUE, TimeUnit.NANOSECONDS);
        } catch (InterruptedException e) {
            // Handle interruption, if necessary
        }

        System.out.println("Executor has terminated.");
    }
}

class MyRunnable implements Runnable {
    @Override
    public void run() {
        // Code to be executed concurrently
        // ...
    }
}

class OtherRunnable implements Runnable {
    @Override
    public void run() {
        // Code to be executed concurrently
        // ...
    }
}
```

`ExecutorService.shutdown` is similar to `Thread.join`: wait for all tasks to finish, then stop all the threads.

`ExecutorService.awaitTermination` waits for shutdown to complete.

`ExecutorService.shutdownNow` interrupts all threads.

### Topic 7 Lab
