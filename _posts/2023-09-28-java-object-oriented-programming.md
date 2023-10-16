---
layout: post
title: Java 面向对象的编程
date: 2023-09-28 07:51 +0800
description:
image: https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-09-28-1695871211.jpg
category:
  - 编程语言
  - JAVA
tags:
  - java
toc: true
comments: true
math: false
mermaid: false
pin: false
sitemap: false
published: true
---

## 面向过程与面向对象的区别

为什么会出现面向对象分析方法？因为现实世界太复杂多变，面向过程的分析方法无法满足。

### Procedure Oriented - 面向过程

采用面向过程必须了解整个过程，每个步骤都有因果关系，每个因果关系都构成了一个步骤，多个步骤就构成了一个系统，因为存在因果关系每个步骤很难分离，非常紧密，当任何一步骤出现问题，将会影响到所有的系统。如：采用面向过程生产电脑，那么他不会分 CPU、主板和硬盘，它会按照电脑的工作流程一次成型。

### Object Oriented - 面向对象

面向对象对会将现实世界分割成不同的单元(对象)，实现各个对象，如果完成某个功能，只需要将各个对象协作起来就可以。

### 面向对象的三大特性

- Encapsulation - 封装
- Inheritance - 继承
- Polymorphism - 多态

## 类与对象的概念

类是对具有共性事物的抽象描述，是在概念上的一个定义，那么如何发现类呢？通常根据名词(概念)来发现类，如在成绩管理系统中：学生、班级、课程、成绩。

- 学生：张三
- 班级：622
- 课程：J2SE
- 成绩：张三成绩

以上“张三”、“622”、“J2SE ”和“张三的成绩”他们是具体存在的，称为对象，也叫实例。也就是说一个类的具体化(实例化)，就是对象或实例。为什么面向对象成为主流技术，主要就是因为更符合人的思维模式，更容易的分析现实世界，所以在程序设计中也采用了面向对象的技术，从软件的开发的生命周期来看，基于面向对象可以分为三个阶段：

- OOA(面向对象的分析)
- OOD(面向对象的设计)
- OOP(面向对象的编程)

我们再进一步的展开，首先看看学生：

- 学生：学号、姓名、性别、地址，班级
- 班级：班级代码、班级名称
- 课程：课程代码、课程名称
- 成绩：学生、课程、成绩

用图形描述我们的概念：

![image](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-09-28-1695871650.png)

以上描述的是类的属性，也就是状态信息，接下来，再做进一步的细化。

![image](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-09-28-1695870918.png)

通过以上分析，可以得出：

类=属性+方法

## 类的定义

类的修饰符 class 类名 extends 父对象名称 implements 接口名称 { 类体:属性和方法组成 }

## 对象的创建和使用

必须 new 出来，才能用。

```java
public class test {
    public static void main(String[] args) {
        //创建一个对象
        Student zhangsan = new Student();
        System.out.println("id=" + zhangsan.id);
        System.out.println("name=" + zhangsan.name);
        System.out.println("sex=" + zhangsan.sex);
        System.out.println("address=" + zhangsan.address);
        System.out.println("age=" + zhangsan.age);
    }
}

class Student {
    //学号
    int id;
    //姓名
    String name;
    //性别
    boolean sex;
    //地址
    String address;
    //年龄
    int age;
}
```

### 各种数据类型的默认值

|   类型   |  默认值  |
| :------: | :------: |
|   byte   |    0     |
|  short   |    0     |
|   int    |    0     |
|   long   |    0L    |
|   char   | '\u0000' |
|  float   |   0.0f   |
|  double  |   0.0d   |
| boolean  |  false   |
| 引用类型 |   null   |

对成员变量进行赋值

```java
        zhangsan.id = 1001;
        zhangsan.name = "张三";
        zhangsan.sex = true;
        zhangsan.address = "北京";
        zhangsan.age = 20;
```

一个类可以创建 N 个对象，成员变量只属于当前的对象(只属于对象，不属于类)，只有通过对象才可以访问成员变量，通过类不能直接访问成员变量。

## 面向对象的封装性

控制对年龄的修改，年龄只能为大于等于 0 并且小于等于 120 的值。

```java
    public void setAge(int studentAge) {
        if (studentAge >= 0 && studentAge <= 120) {
            age = studentAge;
        }
    }
```

从上面的示例，采用方法可以控制赋值的过程，加入了对年龄的检查，避免了直接操纵 student 属性，这就是封装，封装其实就是封装属性，让外界知道这个类的状态越少越好。以上仍然不完善，采用属性仍然可以赋值，如果屏蔽掉属性的赋值，只采用方法赋值，如下:

```java
    private String name;
    //性别
    private boolean sex;
    //地址
    private String address;
    //年龄
    private int age;
```

用 private 来声明成员变量，此时的成员变量只属于 Student，外界无法访问，这样就封装了我们的属性，那么属性只能通过方法访问。

## 构造函数

构造方法主要用来创建类的实例化对象，可以完成创建实例化对象的初始化工作。

构造方法修饰词列表 类名 (方法参数列表)

构造方法修饰词列表:public、proteced、private

- 类的构造方法和普通方法一样可以进行重载

构造方法具有的特点：

- 构造方法名称必须与类名一致
- 构造方法不具有任何返回值类型，关键字 void 也不能加入，加入后就不是构造方法了，就成了普通的方法了
- 任何类都有构造方法，如果没有显示的定义，则系统会为该类定义一个默认的构造器，这个构造器不含任何参数，如果显示的定义了构造器，系统就不会创建默认的不含参数的构造器了。

### 不带参数的构造方法

```java
    public Student() {
        // 在创建对象的时候会执行该构造方法
        // 在创建对象的时候，如果需要做些事情，可以放在构造方法中
        System.out.println("----------Student-------------");
    }
```

### 带参数的构造方法

```java
    public Student(int studentId, String studentName, boolean studentSex, String studentAddress, int studentAge) {
        id = studentId;
        name = studentName;
        sex = studentSex;
        address = studentAddress;
        age = studentAge;
    }
```

在 main 方法中：

```java
        //调用带参数的构造方法对成员变量进行赋值
        Student zhangsan = new Student(1001, "张三", true, "北京", 20);
```

{: .prompt-info }

> 手动建立的构造方法，将会把默认的构造方法覆盖掉。

## 对象和引用

### Java 内存的主要划分

![image](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-09-30-1696032746.png)

#### 栈 Stack

1. 栈描述的是方法执行的内存模型。每个方法被调用都会创建一个栈帧（存储局部变量，操作数，方法出口等）（方法调用的时候在栈中分配空间）
2. JVM 为每个线程创建一个栈，用于存放该线程执行方法的信息（实际参数，局部变量等）
3. 栈属于线程私有，不能实现线程间的共享！
4. 栈的存储特征是：先进后出，后进先出！
5. 栈是由系统自动分配，速度快！栈是一个连续的内存空间

#### 堆 Heap

1. 堆用于存储创建好的对象和数组（数组也是对象）
2. JVM 只有一个堆，被所有线程共享
3. 堆是一个不连续的内存空间，分配灵活，速度慢！

#### 方法区（静态区）

1. JVM 只有一个方法区，被所有线程共享
2. 方法区实际也是堆，只是用于存储类，常量相关的信息！
3. 用来存放程序中永远是不变或唯一的内容（类信息，静态变量，字符串常量等）

### 内存分析详解

- 第一步，执行 main 方法，将 main 方法压入栈，然后 new Student 对象。

```java
//创建一个对象
Student zhangsan = new Student();
```

![image](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-09-30-1696034218.png)

- 第二步，对 student 赋值。

```java
zhangsan.id = 1001;
zhangsan.name = "张三";
zhangsan.sex = true;
zhangsan.address = "北京";
zhangsan.age = 20;
```

![image](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-09-30-1696034938.png)

#### 对象没有更多的引用指向，则变成垃圾

```java
public class test{
    public static void main (String[] args){
        //user是引用，保存内存地址指向堆中的对象
        User user= new User();
        //程序执行到此，user不再指向堆中的对象
        //对象变成了垃圾
        user = null;

        //使用一个空的引用去访问成员
        System.out.println(user.name);//java.lang.NullPointerException 空指针异常
    }
}
```

### 参数传递

#### 值传递

```java
    public static void main(String[] args) {
        int i = 10;
        method1(i);
        System.out.println(i);
    }
```

![image](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-09-30-1696037710.png)

以上代码执行过程，首先在栈中创建 main 方法栈帧，初始化 i 的值为 10，将此栈帧压入栈，接着调用 method()方法，通常是在栈区创建新的栈帧，并赋值 temp 为 1，压入栈中，两栈帧都是独立的，他们之间的值是不能互访的。然后 method()方法栈帧弹出，接下来执行输出 i 值的语句，因为传递的是值，所以不会改变 i 的值，输出结果 i 的值仍然为 10，最后将 main 方法栈帧弹出，main 方法执行完毕。

结论：只要是基本类型，传递过去的都是值

#### 引用传递（地址）

```java
public class test {
    public static void main(String[] args) { //创建一个对象
        Student student = new Student();
        student.id = 1001;
        student.name = "张三";
        student.sex = true;
        student.address = "北京";
        student.age = 20;
        method(student);
        System.out.println("id=" + student.id);
        System.out.println("name=" + student.name);
        System.out.println("sex=" + student.sex);
        System.out.println("address=" + student.address);
        System.out.println("age=" + student.age);
    }

    public static void method(Student temp) {
        temp.name = "李四";
    }
}

class Student {
    //学号
    int id;
    //姓名
    String name;
    //性别
    boolean sex;
    //地址
    String address;
    //年龄
    int age;
}
```

以上执行流程为：

1. 执行 main 方法时，会在栈中创建 main 方法的栈帧，将局部变量 student 引用保存到栈中。
2. 将 Student 对象放到堆区中，并赋值。
3. 调用 method 方法，创建 method 方法栈帧。
4. 将局部变量 student 引用(也就是 student 对象的地址)，传递到 method 方法中，并赋值给 temp。
5. 此时 temp 和 student 引用指向的都是一个对象。
6. 当 method 方法中将 name 属性改为“李四”时，改的是堆中的属性。
7. 所以会影响输出结果，最后的输出结果为:李四。
8. main 方法执行结束，此时的 student 对象还在堆中，我们无法使用了，那么他就变成了“垃圾对象”，java 的垃圾收集器会在某个时刻，将其清除。

**结论：除了基本类型外都是引用地址的传递。**

## this 关键字

this 关键字指的是 **当前调用的对象**，如果有 100 个对象，将有 100 个 this 对象指向各个对象。

this 关键字可以使用在：

- 当局部变量和成员变量重名的时候可以使用 this 指定调用成员变量。
- 通过 this 调用另一个构造方法。

{: .prompt-info }

> this 只能用在构造函数和成员方法内部，还可以应用在成员变量的声明上，static 标识的方法里是不能使用 this 的。

### 当局部变量和成员变量重名的时候可以使用 this 指定调用成员变量

```java
public class test {
    public static void main(String[] args) {
        //创建一个对象
        Student zhangsan = new Student();
        zhangsan.setId(1001);
        System.out.println("id=" + zhangsan.getId()); // 0
    }
}

class Student {
    //学号
    private int id;

    //设置学号
    public void setId(int id) {
        id = id;
    }

    //读取学号
    public int getId() {
        return id;
    }
}
```

输出错误，输出的都是成员变量的默认值，赋值没有起作用，这是为什么?
如:setId(int id) 方法中，我们的赋值语句为 id=id,这样写 Java 无法知道是向成员变量 id 赋值，它遵循“谁近谁优先”，当然局部变量优先了，所以会忽略成员变量。

```java
    public void setId(int id) {
        this.id = id;
    }
```

通过 this 可以取得当前调用对象，采用 this 明确指定了成员变量的名称，所以就不会赋值到局部变量。

## static 关键字

static 修饰符可以修饰:变量、方法和代码块。

- 用 static 修饰的变量和方法，可以采用类名直接访问。
- 用 static 声明的代码块为静态代码块，JVM 第一次使用类的时候，会执行静态代码块中的内容。

### 采用静态变量实现累加器

- static 声明的变量，所有通过该类 new 出的对象，都可以共享，通过该对象都可以直接访问。

```java
public class test {
    public static void main(String[] args) {
        Student student1 = new Student(1001, "张三", true, "北京", 20);
        Student student2 = new Student(1002, "李四", true, "上海", 30);
        System.out.println(student1.getCount()); // 2
        System.out.println(student2.getCount()); // 2
    }
}

class Student {
    //学号
    private int id;
    //姓名
    private String name;
    //性别
    private boolean sex;
    //地址
    private String address;
    //年龄
    private int age;
    //计数器，计算 student 的创建个数
    private static int count;

    public Student(int id, String name, boolean sex, String address, int age) {
        count++;
        this.id = id;
        this.name = name;
        this.sex = sex;
        this.address = address;
        this.age = age;
    }

    public int getCount() {
        return count;
    }
}
```

- static 声明的变量，也可以采用类直接访问，所以我们也称其为类变量。

```java
System.out.println(Student.count);
```

通过以上分析，static 声明的变量会放到方法区中，static 声明的变量只初始化一次，加载类的时候初始化。

### 静态方法中访问实例变量、实例方法或 this 关键字

静态方法中是不能直接调用实例变量、实例方法和使用 this 的，也就是说和实例相关的他都不能直接调用。

```java
    public static void main(String[] args) {
        /*
           在静态区域中不能直接调用成员方法，
           因为静态方法的执行不需要new对象，
           而成员方法必须new对象后才可以执行，
           所以执行静态方法的时候，
           对象还没有创建，所以无法执行成员方法。
        */
        method1();
    }

    public void method1() {
        System.out.println("method1");
    }
```

添加 static 修饰 method，就能在静态区域中直接调用静态方法。

```java
    public static void method1() {
        System.out.println("method1");
    }
```

### 在静态区域中调用成员（实例）变量

#### 错误示范

```java
public class Test {
    private int age = 100;

    public static void main(String[] args) {
        // 在静态区域中无法直接访问成员变量
         System.out.println(age);
        // 可以采用对象的引用调用
         Test t = new Test();
         System.out.println(t.age);
    }
}
```

#### 正确示范

```java
public class Test {
    private static int age = 100;

    public static void main(String[] args) {
        // 直接取得静态变量的值
        System.out.println(age);
        // 可以采用类名+属性名访问
        System.out.println(Test.age);
    }
}
```

#### 在静态区域中使用 this

```java
public class Test {
    private int age = 100;
    public static void main(String[] args) {
        // 因为执行main方法的时候，还没有该对象
        // 所以this也就不存在,无法使用
        System.out.println(this.age);
    }
}
```

{: .prompt-info }

> static 声明的变量或 static 语句块在类加载时就会初始化，而且只初始化一次。

### 解释 main 方法

- public:表示全局所有，其实就是封装性
- static:静态的，也就是说它描述的方法只能通过类调用
- main:系统规定的
- String[] args: 参数类型也是系统规定的

以上为什么是静态方法？这样 java 虚拟机调用起来会更方便，直接拿到类就可以调用 main 方法了，而静态的东西都在方法区中，那么你在静态方法里调用成员属性，他在方法区中无法找到，就算到堆区中查找，可能此成员属性的对象也没有创建，所以静态方法中是不能直接访问成员属性和成员方法的。

## 单例模式初步

什么是设计模式：设计模式是 **可以重复利用的解决方案**。设计模式的提出是在 1995 年，是由 4 位作者提出的，称为 GoF,也就是“四人组”。

设计模式从结构上分为三类:

- 创建型
- 结构性
- 行为型

单例模式的三要素：

- 在类体中需要具有静态的私有的本类型的变量
- 构造方法必须是私有的
- 提供一个公共的静态的入口点方法

```java
public class Test {
    public static void main(String[] args) {
         Singleton s1 = new Singleton(); // Error
        Singleton.getInstance().test();
    }
}

class Singleton {
    //静态私有的本类的成员变量
    private static Singleton instance = new Singleton();
    //私有的构造方法
    private Singleton() {
    }
    //提供一个公共的静态的入口点方法
    public static Singleton getInstance() {
        return instance;
    }
    public void test() {
        System.out.println(" ----------test----------");
    }
}
```

## 类的继承

- 继承是面向对象的重要概念，软件中的继承和现实中的继承概念是一样的。
- 继承是实现软件可重用性的重要手段，如：A 继承 B，A 就拥有了 B 的所有特性，如现实世界中的儿子继承父亲的财产，儿子不用努力就有了财产，这就是重用性。
- java 中只支持类的单继承，也就是说 A 只能继承 B，A 不能同时继承 C。
- java 中的继承使用 extends 关键字，语法格式:
  - [修饰符] class 子类 extends 父类 {}

```java
public class Test {
    public static void main(String[] args) {
        Student student = new Student();
        student.setId(1001);
        student.setName("张三");
        student.setClassesId(10);
        System.out.println("id=" + student.getId());
        System.out.println("name=" + student.getName());
        System.out.println("classid=" + student.getClassesId());

        Employee emp = new Employee();
        emp.setId(1002);
        emp.setName("李四");
        emp.setWorkYear(10);
        System.out.println("id=" + emp.getId());
        System.out.println("name=" + emp.getName());
        System.out.println("workYear=" + emp.getWorkYear());
    }
}

class Person {
    private String name;
    private int id;
    public void setId(int studentId) {
        id = studentId;
    }
    public int getId() {
        return id;
    }
    public void setName(String studentName) {
        name = studentName;
    }
    public String getName() {
        return name;
    }
}

class Student extends Person {
    private int classesId;
    public void setClassesId(int classesId) {
        this.classesId = classesId;
    }
    public int getClassesId() {
        return classesId;
    }
}

class Employee extends Person {
    private int eno;
    private int workYear;
    public void setWorkYear(int workYear) {
        this.workYear = workYear;
    }
    public int getWorkYear() {
        return workYear;
    }
}
```

## 方法的覆盖 - Override

首先看一下方法重载(Overload)的条件：

- **方法名称相同**
- 方法参数类型、个数、顺序至少有一个不同
- **方法的返回类型可以不同**，因为方法重载和返回类型没有任何关系
- **方法的修饰符可以不同**，因为方法重载和修饰符没有任何关系
- 方法重载只出现在 **同一个类中**

方法的覆盖(Override)的条件：

- **必须要有继承关系**
- 覆盖只能出现在子类中，如果没有继承关系，不存在覆盖，只存在重载
- 在子类中被覆盖的方法，必须和父类中的方法 **完全一样**，也就是方法名，返回类型、参数列表，完全一样
- 子类方法的访问权限不能小于父类方法的访问权限
- **子类方法不能抛出比父类方法更多的异常**，但可以抛出父类方法异常的子异常
- 父类的静态方法 **不能** 被子类覆盖
- 父类的私有方法不能覆盖
- **覆盖是针对成员方法**，而非属性

为什么需要覆盖？就是要改变父类的行为。

### 对成员方法覆盖

#### 继承父类方法，不覆盖

```java
public class Test {
    public static void main(String[] args) {

        Student student = new Student();
        student.setId(1001);
        student.setName("张三");
        student.setSex(true);
        student.setAddress("北京");
        student.setAge(20);
        student.setClassesId(10);

        Employee emp = new Employee();
        emp.setId(1002);
        emp.setName("李四");
        emp.setSex(true);
        emp.setAddress("上海");
        emp.setAge(30);
        emp.setWorkYear(10);

        student.printInfo(); // 继承不覆盖
        emp.printInfo(); // 继承不覆盖
    }
}

class Person {

    private int id;
    private String name;
    private boolean sex;
    private String address;
    private int age;

    public void printInfo() {
        System.out.println("id=" + id + ", name=" + name + ",sex=" + sex + ", address=" +
                address + ", age=" + age);
    } //父类方法

    public void setId(int studentId) {
        id = studentId;
    }

    public int getId() {
        return id;
    }

    public void setName(String studentName) {
        name = studentName;
    }

    public String getName() {
        return name;
    }

    public void setSex(boolean studentSex) {
        sex = studentSex;
    }

    public boolean getSex() {
        return sex;
    }

    public void setAddress(String studentAddress) {
        address = studentAddress;
    }

    public String getAddress() {
        return address;
    }

    public void setAge(int studentAge) {
        if (studentAge >= 0 && studentAge <= 120) {
            age = studentAge;
        }
    }
}

class Student extends Person {

    private int classesId;

    public void setClassesId(int classesId) {
        this.classesId = classesId;
    }

    public int getClassesId() {
        return classesId;
    }
}

class Employee extends Person {

    private int workYear;

    public void setWorkYear(int workYear) {
        this.workYear = workYear;
    }

    public int getWorkYear() {
        return workYear;
    }
}
```

![image](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-09-30-1696102002.png)

#### 继承并覆盖父类中的方法

```java
class Person {

    public void printInfo() {
        System.out.println("id=" + id + ", name=" + name + ",sex=" + sex + ", address=" +
                address + ", age=" + age);
    }
}
class Student extends Person {
    //班级编号
    private int classesId;

    public void printInfo() {
        System.out.println("id=" + getId() + ", name=" + getName() + ",sex=" + getSex() + ", address=" +
                getAddress() + ",classesid=" + classesId);
    }
}

class Employee extends Person {
    //工作年限
    private int workYear;

    public void printInfo() {
        System.out.println("id=" + getId() + ", name=" + getName() + ",sex=" + getSex() + ", address=" +
                getAddress() + ", workYear=" + workYear);
    }
}
```

![image](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-09-30-1696102653.png)

以上子类对父类的方法进行了覆盖，改变了父类的行为，当我们 new 子类的时候，它不会再调用父类的方法了，而直接调用子类的方法，所以我们就完成了对父类行为的扩展。

#### 注意

```java
        //此种写法是错误的，Person不是Student
        Student person_student = new Person();
        //此种写法是正确的，Student是Person
        Person person_student = new Student();

        //编译出错，因为Person看不到子类Student的属性
        person_student.setClassesId(10);

        //编译出错，因为Person看不到子类Employee的属性
        person_emp.setWorkYear(10);
```

#### 父类的引用指向子类的对象

```java
        Person person_student = new Student();
        Person person_emp = new Employee();
        Person person = new Person();
```

多态其实就是多种状态，overload(重载)是多态的一种，属于编译期绑定，也就是静态绑定(前期绑定)，override 是运行期间绑定(后期绑定)。

多态存在的条件:

- 有继承
- 有覆盖
- 父类指向子类的引用

### 对静态方法覆盖

```java
public class Test {
    public static void main(String[] args) {
        Person person_student = new Student();
        print(person_student); // "------------Person-------------"
        Person person_emp = new Employee();
        print(person_emp); // "------------Person-------------"
    }

    private static void print(Person person) {
        person.printInfo();
    }
}

class Person {
    public void printInfo() {
        System.out.println("------------Person-------------");
    }
}

class Student extends Person {

    public void printInfo() {
        System.out.println("------------Student-------------");
    }
}

class Employee extends Person {

    public void printInfo() {
        System.out.println("------------Employee-------------");
    }
}
```

![image](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-09-30-1696106033.png)

结论：

- 静态方法不存在多态的概念，**多态和成员方法相关**。
- 静态方法只属于某一个类，**声明的是哪个类就调用的是哪个类的静态方法**，和子类没有关系。
- 不像覆盖了成员方法，**new 谁调谁**。

#### 为什么覆盖成员方法可以构成多态(多种状态)？

主要是运行期的动态绑定(动态绑定构成多态)，静态绑定的含义是在编译成`.class`文件(字节码)的时候已经确定该调用哪个方法。

## super 关键字

super 关键字的作用：

- 调用父类的构造方法
- 调用父类的成员方法

{: .prompt-info}

> super 只能应用在成员方法和构造方法中，不能应用在静态方法中(和 this 是一样的)，如果在构造方法中使用必须放在第一行。

为什么会有 super 关键字?

- 因为子类必须要调用父类的构造方法，先把父类构造完成，因为子类依赖于父类，没有父，也就没有子。
- 有时需要在子类中显示的调用父类的成员方法。

那么我们以前为什么没有看到 super，而且我们也有继承，如：Student 继承了 Person?

- 因为子类中我们没有显示的调用构造方法，那么他会默认调用父类的无参构造方法，此种情况下如果父类中没有无参构造方法，那么编译时将会失败。

{: .prompt-info}

> 构造方法不存在覆盖的概念，构造方法可以重载

### 调用默认构造方法

```java
class Student extends Person {
    public Student() {
        //显示调用父类的构造方法
        //调用父类的无参的构造函数
        super();
        System.out.println("--------Student----------");
    }
}
```

{: .prompt-info}

> 必须将 super 放到构造函数的第一语句，必须先创建父类，才能创建子类。

关于构造方法的执行顺序，先执行父类的再执行子类的，必须完成父类的所有初始化，才能创建子类。

### 调用带参数的构造方法

```java
class Person {

    public Person(int id, String name, boolean sex, String address, int age) {
        this.id = id;
        this.name = name;
        this.sex = sex;
        this.address = address;
        this.age = age;
    }

    //学号
    private int id;
    //姓名
    private String name;
    //性别
    private boolean sex;
    //地址
    private String address;
    //年龄
    private int age;
}

class Student extends Person {
    //班级编号
    private int classesId;

    public Student(int id, String name, boolean sex, String address, int age, int classesId) {
        //手动调用调用带参数的构造函数
        super(id, name, sex, address, age);
        this.classesId = classesId;
    }
}
```

### 采用 super 调用父类的方法

```java
class Student extends Person {

    public void printInfo() {
        //采用 super 调用父类的方法
        super.printInfo(); // System.out.println("------------Person-------------");
        System.out.println("------------Student-------------");
    }
}
```

## final 关键字

final 表示不可改变。

- 采用 final 修饰的类不能被继承

```java
final class A {
    public void test1() {
    }
}

    //不能继承A1，因为A采用final修饰了
    class B extends A {

    public void test2() {
    }
}
```

- 采用 final 修饰的方法不能被覆盖

```java
class A1 {
    public final void test1() {
    }
}

class B1 extends A1 {
    //覆盖父类的方法，改变其行为
    //因为父类的方法是final修饰的，所以不能覆盖
    public void test1() {
    }
}
```

- 采用 final 修饰的变量不能被修改

```java
public class Test {
    private static final long CARD_NO = 878778878787878L;
    public static void main(String[] args) {
        //不能进行修改，因为CARD_NO采用final修饰了
        CARD_NO = 99999999999999L;
    }
}
```

- final 修饰的变量必须显示初始化

```java
//如果是final修饰的变量必须初始化
private static final long CARD_NO = 0L;
```

- **如果修饰的引用，那么这个引用只能指向一个对象，也就是说这个引用不能再次赋值，但被指向的对象是可以修改的**

```java
public class Test {
    public static void main(String[] args) {
        Person p1 = new Person();
        //可以赋值
        p1.name = "张三";
        System.out.println(p1.name);
        final Person p2 = new Person();
        p2.name = "李四";
        System.out.println(p2.name);

        //不能编译通过
        //p2采用final修饰，主要限制了p2指向堆区中的地址不能修改(也就是p2只能指向一个对象)
        //p2指向的对象的属性是可以修改的
        p2 = new Person();
    }
}

class Person {
    String name;
}
```

![image](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-10-01-1696118453.png)

- 构造方法不能被 final 修饰
- **会影响 JAVA 类的初始化：final 定义的静态常量调用时不会执行 java 的类初始化方法，也就是说不会执行 static 代码块等相关语句，这是由 java 虚拟机规定的。我们不需要了解的很深，有个概念就可以了。**

## 抽象类

看我们以前示例中的 Person、Student 和 Employee，从我们使用的角度来看主要对 Student 和 Employee 进行实例化，Person 中主要包含了一些公共的属性和方法，而 Person 我们通常不会实例化，所以我们可以把它定义成抽象的：

- 在 java 中采用 abstract 关键字定义的类就是抽象类，采用 abstract 关键字定义的方法就是抽象方法
- 抽象的方法只需在抽象类中，提供声明，不需要实现
- 如果一个类中含有抽象方法，那么这个类必须定义成抽象类
- 如果这个类是抽象的，那么这个类被子类继承，抽象方法必须被重写。如果在子类中不复写该抽象方法，那么必须将此类再次声明为抽象类
- 抽象的类是不能实例化的，就像现实世界中人其实是抽象的，张三、李四才是具体的
- 抽象类不能被 final 修饰
- 抽象方法不能被 final 修饰，因为抽象方法就是被子类实现的。抽象类中可以包含方法实现，可以将一些公共的代码放到抽象类中，另外在抽象类中可以定义一些抽象的方法，这样就会存在一个约束，而子类必须实现我们定义的方法，如：teacher 必须实现 printInfo()方法，Student 也必须实现 printInfo()方法，方法名称不能修改，必须为 printInfo()，这样就能实现多态的机制，有了多态的机制，我们在运行期就可以动态的调用子类的方法。所以在运行期可以灵活的互换实现。

![image](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-10-01-1696119266.png)

### 采用 abstract 声明抽象类

```java
public class Test {
    public static void main(String[] args) {
        //不能实例化抽象类
        //抽象类是不存在，抽象类必须有子类继承
        Person p = new Person();
        //以下使用是正确的，因为我们new的是具体类
        Person p1 = new Employee();
        p1.setName("张三");
        System.out.println(p1.getName());
    }
}

//采用abstract定义抽象类
//在抽象类中可以定义一些子类公共的方法或属性
//这样子类就可以直接继承下来使用了，而不需要每个
//子类重复定义
abstract class Person {
    private String name;

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    //此方法各个子类都可以使用
    public void commonMethod1() {
        System.out.println("---------commonMethod1-------");
    }
}

class Employee extends Person {
}

class Student extends Person {
}
```

{: .prompt-info}

> 抽象类中可以没有抽象方法

### 抽象的方法只需在抽象类中，提供声明，不需要实现，起到了一个强制的约束作用，要求子类必须实现

```java
abstract class Person {

    //采用abstract定义抽象方法
    //如果有一个方法为抽象的，那么此类必须为抽象的
    //如果一个类是抽象的，并不要求具有抽象的方法
    public abstract void printInfo();
}
class Employee extends Person {
    //必须实现抽象的方法
    public void printInfo() {
        System.out.println(" Employee.printInfo()");
    }
}

class Student extends Person {
    //必须实现抽象的方法
    public void printInfo() {
        System.out.println(" Student.printInfo()");
    }
}
```

### 如果这个类是抽象的，那么这个类被子类继承，抽象方法必须被覆盖。如果在子类中不覆盖该抽象方法 ，那么必须将此方法再次声明为抽象方法

```java
public class Test {
    public static void main(String[] args) {
        //此时不能再new Employee了
        Person p = new Employee();
    }
}
abstract class Person {
    //采用abstract定义抽象方法
    public abstract void printInfo();
}

abstract class Employee extends Person {
    //再次声明该方法为抽象的
    public abstract void printInfo();
}
```

### 抽象类不能被 final 修饰

```java
//两个关键字是矛盾的
final abstract class Person {}
```

### 抽象方法不能被 final 修饰

```java
//不能采用 final 修饰抽象的方法 //这两个关键字存在矛盾
public final abstract void printInfo();
```

## 接口

接口我们可以看作是抽象类的一种特殊情况，在接口中只能定义抽象的方法和常量。

- 在 java 中接口采用 interface 声明
- 接口中的方法默认都是 public abstract 的，不能更改

```java
//采用 interface 定义接口
//定义功能，没有实现
//实现委托给类实现
interface StudentManager {
    //正确，默认public abstract等同 public abstract void addStudent(int id,String name);
    public void addStudent(int id, String name);

    //正确
    public abstract void addStudent(int id, String name);

    //正确，可以加入public修饰符，此种写法较多
    public void delStudent(int id);

    //正确，可以加入abstract，这种写法比较少
    public abstract void modifyStudent(int id, String name);

    //编译错误，因为接口就是让其他人实现
    //采用private就和接口原本的定义产生矛盾了
    private String findStudentById(int id);
}
```

- 接口中的变量默认都是 public static final 类型的，不能更改，所以必须显示的初始化

```java
public class Test {
    public static void main(String[] args) {
        //不能修改，因为 final 的
        StudentManager.YES = "abc";
        System.out.println(StudentManager.YES);
    }
}

interface StudentManager {
    //正确，默认加入 public static final
    String YES = "yes";
    //正确, 开发中一般就按照下面的方式进行声明
    public static final String NO = "no";
    //错误，必须赋值，因为是final的
    int ON;
    //错误，不能采用private声明
    private static final int OFF = -1;
}
```

- 接口不能被实例化，接口中没有构造函数的概念

```java
public class Test {
    public static void main(String[] args) {
        //接口是抽象类的一种特例，只能定义方法和变量，没有实现
        //所以不能实例化
        StudentManager studentManager = new StudentManager();
    }
}

interface StudentManager {
    public void addStudent(int id, String name);
}
```

- 接口之间可以继承，但接口之间不能实现

```java
interface inter1 {
    public void method1();
    public void method2();
}

interface inter2 {
    public void method3();
}

//接口可以继承
interface inter3 extends inter1 {
    public void method4();
}

//接口不能实现接口
//接口只能被类实现
interface inter4 implements inter2 {
    public void method3();
}
```

- 接口中的方法只能通过类来实现，通过 implements 关键字
- 如果一个类实现了接口，那么接口中所有的方法必须实现

```java
public class Test {
    public static void main(String[] args) {

        //Iter1Impl实现了Inter1接口
        //所以它是Inter1类型的产品
        //所以可以赋值
        Inter1 iter1 = new Iter1Impl();
        iter1.method1();

        //Iter1Impl123 实现了Inter1接口
        //所以它是Inter1类型的产品
        //所以可以赋值
        iter1 = new Iter1Impl123();
        iter1.method1();

        //可以直接采用Iter1Impl来声明类型
        //这种方式存在问题
        //不利于互换，因为面向具体编程了
        Iter1Impl iter1Impl = new Iter1Impl();
        iter1Impl.method1();

        //不能直接赋值给iter1Impl
        //因为Iter1Impl123不是Iter1Impl类型
        iter1Impl = new Iter1Impl123();
        iter1Impl.method1();
    }
}

//接口中的方法必须全部实现
class Iter1Impl implements Inter1 {
    public void method1() {
        System.out.println("method1");
    }

    public void method2() {
        System.out.println("method2");
    }

    public void method3() {
        System.out.println("method3");
    }
}

class Iter1Impl123 implements Inter1 {
    public void method1() {
        System.out.println("method1_123");
    }
    public void method2() {
        System.out.println("method2_123");
    }
    public void method3() {
        System.out.println("method3_123");
    }
}

//定义接口
interface Inter1 {
    public void method1();

    public void method2();

    public void method3();
}
```

- 一类可以实现多个接口

```java
public class Test {
    public static void main(String[] args) {

        //可以采用Inter1定义
        Inter1 inter1 = new InterImpl();
        inter1.method1();
        //可以采用Inter2定义
        Inter2 inter2 = new InterImpl();
        inter2.method2();
        //可以采用Inter3定义
        Inter3 inter3 = new InterImpl();
        inter3.method3();
    }
}

//实现多个接口，采用逗号隔开
//这样这个类就拥有了多种类型
//等同于现实中的多继承
//所以采用java中的接口可以实现多继承
//把接口粒度划分细了，主要使功能定义的含义更明确
//可以采用一个大的接口定义所有功能，替代多个小的接口
//但这样定义功能不明确，粒度太粗了
class InterImpl implements Inter1, Inter2, Inter3 {
    public void method1() {
        System.out.println("----method1-------");
    }

    public void method2() {
        System.out.println("----method2-------");
    }

    public void method3() {
        System.out.println("----method3-------");
    }
}

interface Inter1 {
    public void method1();
}

interface Inter2 {
    public void method2();
}

interface Inter3 {
    public void method3();
}
```

### 接口的进一步应用

在 java 中接口其实描述了类需要做的事情，类要遵循接口的定义来做事，使用接口到底有什么本质的好处?

可以归纳为三点:

- 采用接口明确的声明了它所能提供的服务
- 解决了 Java 单继承的问题
- 实现了可接插性(重要)

示例：完成学生信息的增删改操作，系统要求适用于多个数据库，如：适用于 MySQL 和 Oracle

- 第一种方案，不使用接口，每个数据库实现一个类：
  ![image](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-10-01-1696123484.png)

#### Oracle 的实现

```java
public class StudentOracleImpl {
    public void add(int id, String name) {
        System.out.println("StudentOracleImpl.add()");
    }

    public void del(int id) {
        System.out.println("StudentOracleImpl.del()");
    }

    public void modify(int id, String name) {
        System.out.println("StudentOracleImpl.modify()");
    }
}
```

需求发生变化了，客户需要将数据移植到 Mysql 上

```java
public class StudentMysqlImpl {
    public void addStudent(int id, String name) {
        System.out.println("StudentMysqlImpl.addStudent()");
    }

    public void deleteStudent(int id) {
        System.out.println("StudentMysqlImpl.deleteStudent()");
    }

    public void udpateStudent(int id, String name) {
        System.out.println("StudentMysqlImpl.udpateStudent()");
    }
}
```

调用以上两个类完成向 Oracle 数据库和 Mysql 数据存储数据

```java
public class StudentManager {

    public static void main(String[] args) {

        //对 Oracle 数据库的支持
        StudentOracleImpl studentOracleImpl = new StudentOracleImpl();
        studentOracleImpl.add(1, "张三");
        studentOracleImpl.del(1);
        studentOracleImpl.modify(1, "张三");

        //需要支持 Mysql 数据库
        StudentMysqlImpl studentMysqlImpl = new StudentMysqlImpl();
        studentMysqlImpl.addStudent(1, "张三");
        studentMysqlImpl.deleteStudent(1);
        studentMysqlImpl.udpateStudent(1, "张三");
    }
}
```

以上代码不能灵活的适应需求，当需求发生改变需要改动的代码量太大，这样可能会导致代码的冗余，另外可能会导致项目的失败，为什么会导致这个问题，在开发中没有考虑到程序的扩展性，就是一味的实现，这样做程序是不行的，所以大的项目比较追求程序扩展性，有了扩展性才可以更好的适应需求。

- 第二种方案，使用接口
  ![image](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-10-01-1696124242.png)

```java
public class Student4OracleImpl implements IStudent {
    public void add(int id, String name) {
        System.out.println("Student4OracleImpl.add()");
    }

    public void del(int id) {
        System.out.println("Student4OracleImpl.del()");
    }

    public void modify(int id, String name) {
        System.out.println("Student4OracleImpl.modify()");
    }
}

public class Student4MysqlImpl implements IStudent {
    public void add(int id, String name) {
        System.out.println("Student4MysqlImpl.add()");
    }

    public void del(int id) {
        System.out.println("Student4MysqlImpl.del()");
    }

    public void modify(int id, String name) {
        System.out.println("Student4MysqlImpl.modify()");
    }
}

public class StudentService {
    public static void main(String[] args) {
        doCrud(new Student4MysqlImpl());
    }

    //此种写法没有依赖具体的实现
    //而只依赖的抽象，就像你的手机电池一样：你的手机只依赖电池(电池是一个抽象的事物)
    //而不依赖某个厂家的电池(某个厂家的电池就是具体的事物了)
    //因为你依赖了抽象的事物，每个抽象的事物都有不同的实现
    //这样你就可以利用多态的机制完成动态绑定，进行互换，使程序具有较高的灵活
    //我们尽量遵循面向接口(抽象)编程,而不要面向实现编程
    public static void doCrud(IStudent istudent) {
        istudent.add(1, "张三");
        istudent.del(1);
        istudent.modify(1, "张三");
    }

    //以下写法不具有扩展性
    //因为它依赖了具体的实现
    //建议不要采用此种方法，此种方案是面向实现编程，就依赖于具体的东西了
    public static void doCrud(Student4OracleImpl istudent) {
        istudent.add(1, "张三");
        istudent.del(1);
        istudent.modify(1, "张三");
    }
}
```

## 多态

多态其实就是多种状态的含义，如我们方法重载，相同的方法名称可以完成不同的功能，这就是多态的一种表现，此时成为静态多态。另外就是我们将学生保存到数据的示例，当我们调用 Istudent 接口中的方法，那么 java 会自动调用实现类的方法，如果是 Oracle 实现就调用 Oracle 的方法，如果是 MySql 实现就调用 MySql 中的方法，这是在运行期决定的。多态的条件是:有继承或实现，有方法覆盖或实现，父类对象(接口)指向子类对象。

### 接口和抽象类的区别?

- 接口描述了方法的特征，不给出实现，一方面解决 java 的单继承问题，实现了强大的可接插性
- 抽象类提供了部分实现，抽象类是不能实例化的，抽象类的存在主要是可以把公共的代码移植到抽象类中
- 面向接口编程，而不要面向具体编程(面向抽象编程，而不要面向具体编程)
- 优先选择接口(因为继承抽象类后，此类将无法再继承，所以会丧失此类的灵活性)

### 类之间的关系

1. 泛化关系，类和类之间的继承关系及接口与接口之间的继承关系。
2. 实现关系，类对接口的实现。
3. 关联关系，类与类之间的连接，一个类可以知道另一个类的属性和方法，在 java 中使用成员变量体现。
4. 聚合关系，是关联关系的一种，是较强的关联关系，是整体和部分的关系，如：汽车和轮胎，它与关联关系不同，关联关系的类处在同一个层次上，而聚合关系的类处在不平等的层次上，一个代表整体，一个代表部分，在 java 语言中使用实例变量体现。
5. 合成关系，是关系的一种，比聚合关系强的关联关系 ，如：人和四肢，整体对象决定部分对象的生命周期，部分对象每一时刻只与一个对象发生合成关系，在 java 语言中使用实例变量体现。
6. 依赖关系，依赖关系是比关联关系弱的关系，在 java 语言中体现为返回值，参数，局部变量和静态方法调用。

## is-a、is-like-a、has-a

### is-a

```java
public class Animal {
    public void method1();
}
public class Dog extends Animal {
     //Dog is a Animal
}
```

### is-like-a

```java
public interface I {
    public void method1();
}
public class A implements I{
    //A is like a I;
    public void method1() {
        //实现
    }
}
```

### has-a

```java
public class A {
    //A has a B;
    private B b;
}
public class B {}
```

## Object 类

- Object 类是所有 Java 类的根基类
- 如果在类的声明中未使用 extends 关键字指明其基类，则默认基类为 Object 类

如：

```java
public class User {}
```

相当于

```java
public class User extends Object {}
```

### toString()

SUN 在 Object 类中设计 toString 方法的目的：返回 java 对象的字符串表示形式。在现实的开发过程中，Object 里的 toString 方法已经不够用了，因为 Object 的 toString 方法实现的结果不尽人意。

**Object 中的 toString 方法就是要被重写的。**

SUN 是这样实现 toString 方法的：

```java
getClass().getName() + '@' + Integer.toHexString(hashCode());
```

Object 中的 toString 方法返回：类名@java 对象的内存地址经过哈希算法得出的 int 类值再转换成十六进制。这个输出结果可以等同看作 java 对象在堆中的内存地址。

在进行 String 与其它类型数据的连接操作时，如：

```java
System.out.println(student);
```

它自动调用该对象的 toString()方法。

```java
class Person {
    String name;
    int age;

 Person(String name, int age) {
    this.name = name;
    this.age = age;
    }

 // 重写toString方法
 // 根据项目的需求而定
 // 需求规定，显示格式：Person[name=刘德华，age=50]
 public String toString() {
    return "Person[name=" + name + ",age=" + age + "]";
    }
}

public class Test {

    // 入口
    public static void main(String[] args) {
    // 创建一个Object类型的对象
    Object o1 = new Object();

    // 调用toString方法
    String oStr = o1.toString();
    System.out.println(oStr);

    // 创建一个Person类型的对象
    Person p1 = new Person("刘德华", 50);

    // 调用toString方法
    String pStr = p1.toString();
    System.out.println(pStr);

    Person p2 = new Person("巩俐", 50);
    System.out.println(p2.toString());

    // print方法后面括号中如果是一个引用类型，会默认调用引用类型的toString方法
    System.out.println(p2);
    }
}
```

### finalize

垃圾回收器(Garbage Collection)，也叫 GC，垃圾回收器主要有以下特点：

- 当对象不再被程序使用时，垃圾回收器将会将其回收
- 垃圾回收是在后台运行的，我们无法命令垃圾回收器马上回收资源，但是我们可以
  告诉他，尽快回收资源(System.gc 和 Runtime.getRuntime().gc())
- 垃圾回收器在回收某个对象的时候，首先会调用该对象的 finalize 方法
- GC 主要针对堆内存
- 单例模式的缺点

当垃圾收集器将要收集某个垃圾对象时将会调用 finalize，建议不要使用此方法，因为此方法的运行时间不确定，如果执行此方法出现错误，程序不会报告，仍然继续运行。

### ==与 equals 方法

#### ==

- == 可以比较基本类型和引用类型，等号比较的是值。
- == 两边如果是引用类型，则比较内存地址，地址相同则是 true,反之则 false

```java
    public static void main(String[] args) {
        int a = 100;
        int b = 100;

        //可以成功比较
        //采用等号比较基本它比较的就是具体的值
        System.out.println((a == b) ? "a==b" : "a!=b");

        Person p1 = new Person();
        p1.id = 1001;
        p1.name = "张三";
        Person p2 = new Person();
        p2.id = 1001;
        p2.name = "张三";
        //输出为 p1!=p2
        //采用等号比较引用类型比较的是引用类型的地址(地址也是值)
        //这个是不符合我们的比较需求的
        //我们比较的应该是对象的具体属性，如：id相等，或id和name相等
        System.out.println((p1 == p2) ? "p1==p2" : "p1!=p2");
        Person p3 = p1;

        //输出为 p1==p3
        //因为p1和p3指向的是一个对象，所以地址一样
        //所以采用等号比较引用类型比较的是地址
        System.out.println((p1 == p3) ? "p1==p3" : "p1!=p3");
    }
```

#### equals

Object 中的 equals 方法源码：

```java
    public boolean equals (Object obj) {
        return (this == obj);
    }
```

- o1.equals(o2); o1 是 this,o2 是 obj。
- Object 中的 equals 方法比较的是两个引用的内存地址。
- Java 对象中 equals 方法的设计目的：判断两个对象是否一样。

在现实的业务逻辑当中，不应该比较内存地址，应该比较内容。**所以 Object 中的 equals 方法也要重写。**

根据需求规定重写 equals 方法：

- 需求规定：如果身份证号一致，并且名字也一致，则代表是同一个人。

```java
class Star {
    //身份证号
    int id;
    //姓名
    String name;

    //Constructor
    public Star(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj instanceof Star) {
            Star s = (Star) obj;
            if (s.id == id && s.name.equals(this.name)) {
                return true;
            }
        }
        return false;
    }
}

public class Test01 {
    public static void main(String[] args) {
        Object o1 = new Object();
        Object o2 = new Object();

        boolean b1 = o1.equals(o2);
        System.out.println(b1); // false

        Star s1 = new Star(100, "黄晓明");
        Star s2 = new Star(100, "黄晓明");

        System.out.println(s1.equals(s2)); // true
    }
}
```

在 java 中比较两个字符串是否一致，不能用“==”，只能调用 String 类的 equals 方法。

```java
public class Test {
    public static void main(String[] args) {
        String s1 = new String("ABC");
        String s2 = new String("ABC");

        System.out.println(s1 == s2); //false

        //String已经重写了Object中的equals方法，比较的是内容
        System.out.println(s1.equals(s2)); //true
    }
}
```

## 包和 import

### 包

- 包最好采用小写字母
- 包的命名应该有规则，不能重复，一般采用公司网站逆序
  - 如：com.hsbc.oa.system;
  - 以上包含意义：hsbc 公司开发 oa 项目，system 是 oa 项目中其中一个模块！
  - package 定义的全格式：公司域名倒叙.项目名.模块名
- **package 必须放到所有语句的第一行**
- 完整的类名是带有包名的
- 带有 package 语句的 java 源文件必须这样编译：
  - `javac -d 生成路径 java源文件路径`

### import

- 采用 import 引入需要使用的类
- 可以采用\*通配符引入包下的所有类
- 如果都在同一个包下就不需要 import 引入了

## 访问控制权限

|  修饰符   | 类的内部 | 同一个包里 | 子类 | 任何地方 |
| :-------: | :------: | :--------: | :--: | :------: |
|  private  |    Y     |     N      |  N   |    N     |
|  default  |    Y     |     Y      |  N   |    N     |
| protected |    Y     |     Y      |  Y   |    N     |
|  public   |    Y     |     Y      |  Y   |    Y     |

## 内部类

在一个类的内部定义的类，称为内部类。

分类：

- 静态内部类
- 成员内部类
- 局部内部类
- 匿名内部类

### 静态内部类

- 静态内部类可以等同看作静态变量
- 静态内部类可以直接访问外部类的静态数据，无法直接访问成员

```java
public class OuterClass {
    //静态变量
    private static String s1 = "A";
    //成员变量
    private String s2 = "B";

    //静态方法
    private static void m1() {
        System.out.println("static's m1 method execute!");
    }

    //成员方法
    private void m2() {
        System.out.println("m2 method execute");
    }

    //静态内部类
    //可以用访问控制权限的修饰符修饰
    //public, protected, private, default
    static class InnerClass {
        //静态方法
        public static void m3() {
            System.out.println(s1);
            m1();
        }

        //成员方法
        public void m4() {
            System.out.println(s1);
            m1();
        }
    }

    public static void main(String[] args) {
        //执行m3
        OuterClass.InnerClass.m3();
        //执行m4
        InnerClass inner = new OuterClass.InnerClass();
        inner.m4();
    }
}
```

### 成员内部类

- 成员内部类可以等同看作成员变量
- 成员内部类中不能有静态声明
- 成员内部类可以访问外部类所有的数据

```java
public class OuterClass {
    //静态变量
    private static String s1 = "A";
    //成员变量
    private String s2 = "B";

    //静态方法
    private static void m1() {
        System.out.println("static's m1 method execute!");
    }

    //成员方法
    private void m2() {
        System.out.println("m2 method execute");
    }

    //成员内部类
    //可以用访问控制权限的修饰符修饰
    //public, protected, private, default
    class InnerClass {
        //成员方法
        public void m4() {
            System.out.println(s1);
            m1();
            System.out.println(s2);
            m2();
        }
    }
    //入口
    public static void main(String[] args) {
        //创建外部类对象
        OuterClass oc = new OuterClass();
        InnerClass inner = oc.new InnerClass();
        inner.m4();
    }
}
```

### 局部内部类

- 局部内部类，等同于局部变量

**重点**：局部内部类在访问局部变量的时候，局部变量必须使用 final 修饰。

```java

public class OuterClass {
    public void m1() {
        //局部变量
        final int i = 10;
        //局部内部类
        //局部内部类不能用访问控制权限修饰符修饰，这个类只在m1方法有效
        class InnerClass {
            //内部类不能有静态声明
            //成员方法
            public void m2() {
                System.out.println(i);// 10
            }
        }
        //调用m2，必须在m1方法中new一个InnerClass对象出来，再用这个对象去调用m2
        InnerClass inner = new InnerClass();
        inner.m2();
    }

    public static void main(String[] args) {
        OuterClass oc = new OuterClass();
        oc.m1();
    }
}
```

### 匿名内部类

匿名内部类：指的是类没有名字。

```java
public class Test01 {

    //静态方法
    public static void test(CustomerService cs) {
        cs.logout();
    }

    public static void main(String[] args) {
        //调用test方法
        test(new CustomerServiceImpl());

        //使用匿名内部类的方式执行test方法
        //缺点：无法重复使用
        //优点：少定义一个类
        test(new CustomerServiceImpl() {
            public void logout() {
                System.out.println("匿名内部类的系统已经安全退出");
            }
        });
    }
}

//Interface
interface CustomerService {
    //退出系统
    void logout();
}

//编写一个类实现CustomerService接口
class CustomerServiceImpl implements CustomerService {
    public void logout() {
        System.out.println("系统已经安全退出");
    }// 使用匿名内部类就不需要再写这个类了
}
```

{% include busuanzi.html %}
