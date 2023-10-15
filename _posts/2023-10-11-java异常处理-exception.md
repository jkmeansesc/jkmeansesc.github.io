---
layout: post
title: Java异常处理 - Exception
date: 2023-10-11 05:32 +0800
description:
image: https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-10-10-1696973814.jpg
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
sitemap: true
published: true
---
## 异常的基本概念

- 异常模拟的是现实世界中“不正常”的事件。
- Java 中采用“类”去模拟异常。
- 类是可以创建对象的.
  - `NullPointerException e = 0x1234l;`
  - `e`是引用类型，`e`中保存的内存地址指向堆中的“对象”。
  - 这个对象一定是`NullPointerException`类型。
  - 这个对象就表示真实存在的异常事件。
  - `NullPointerException`是一类异常。
  - “抢劫”就是一类异常--->类.
  - ”张三被抢劫”就是一个异常事件--->对象.

## 异常机制的作用

程序发生异常事件之后，为我们输出详细的信息。我们可以把这个信息，再进行处理以下告诉用户.

```java
public class Test {
    public static void main(String[] args) {
        int a = 10;
        int b = 0;
        int c = a / b; // `ArithmeticException e = 0x1234;`
        // 上面的代码出现了异常， “没有处理”，下面的代码不会执行，直接退出了JVM
        System.out.println("Hello World");
    }
}
```

- 以上程序编译通过了，但是运行时出现了异常，表示发生了某个异常事件。
- JVM 向控制台输入如下的信息:
  - Exception in thread "main" java.lang.ArithmeticException: / by zero
    at com.demo.ExceptionTest01.main(ExceptionTest01.java:24)
- 本质:程序执行过程中发生了算数异常这个事件，JVM 为我们创建了一个 ArithmeticException 类型的对象，并且这个对象中包含了详细的异常信息，并且 JVM 将这个对象中的信息输出到控制台。

异常处理机制使程序更加健壮.

```java
public class Test {
    public static void main(String[] args) {
        int a = 10;
        int b = 2;
        if (b != 0) {
            int c = a / b;
            System.out.println(a + "/" + b + "=" + c);
        } else {
            System.out.println("除数不能为0");
        }
    }
}
```

## 异常的分类

![已检查异常](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-10-10-1696951004.jpeg)

红色为已检查异常 (Checked Exceptions):

- 已检查异常是指 Java 编译器和 Java 虚拟机在编译时会检查在方法中可能抛出的异常，必须在方法内部捕获（告诉程序在遇到异常时怎么做）或在方法的 throws 子句中声明，否则编译无法通过。

绿色为未检查异常 (Unchecked Exceptions):

- 未检查异常是指不太可能被恢复的异常，例如空指针、除零等。
- 这些异常通常不需要被显式捕获或声明，因为它们通常表示程序中出现了严重的错误，而不是正常的异常情况.
  ![未检查异常](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-10-10-1696958646.png)

## try、catch 和 finally 捕捉处理异常

具体格式如下:

```java
try {
    // Your code that may throw exceptions
} catch (OneException e) {
    // Handle OneException
} catch (TwoException e) {
    // Handle TwoException
} finally {
    // Code to be executed regardless of exceptions
}
```

- try 中包含了可能产生异常的代码。
- try 后面是 catch，catch 可以有一个或多个，catch 中是需要捕获的异常。
  - 但是从上到下 catch，必须从小类型异常到大异常类型进行捕捉。
  - try...catch...中最多执行 1 个 catch 语句块，执行结束之后 try...catch...就结束了。
- 当 try 中的代码出现异常时，出现异常下面的代码不会执行，马上会跳转到相应的 catch 语句块中，如果没有异常不会跳转到 catch 中。
- finally 表示，不管是出现异常，还是没有出现异常，finally 里的代码都执行，finally 和 catch 可以分开使用，但 finally 必须和 try 一块使用。

代码示例：

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class ExceptionTest06 {
    public static void main(String[] args) {
        FileInputStream file = null;
        try {
            //程序执行到此处发生了FileNotFoundException类型的异常
            //JVM会自动创建一个FileNotFoundException类型的对象，将该对象的内存地址赋值给catch语句块中的e变量
            file = new FileInputStream("abc");

            //上面的代码出现了异常，try语句块的代码不再继续执行，直接进入catch语句块中执行
            System.out.println("TTTTT");
        } catch (FileNotFoundException e) {
            System.out.println("读取的文件不存在！");
        }
        try {
            file.read();
        } catch (IOException e) {
            System.out.println("其他IO异常");
        }
        System.out.println("ABC");
    }
}
```

## getMessage 和 printStackTrace()

如何取得异常对象的具体信息，常用的方法主要有两种:

- 取得异常描述信息: getMessage()
- 取得异常的堆栈信息(比较适合于程序调试阶段): printStackTrace();

## finally 关键字

- finally 在任何情况下都会执行，通常在 finally 里关闭资源（只有在执行 finnaly 语句块之前退出了 JVM，则 finally 语句块不会执行）。

- finally 语句块可以直接和 try 语句块连用 try...finally...

```java
import java.io.*;

public class ExceptionTest07 {
    public static void main(String[] args) {
        try {
            FileInputStream fis = new FileInputStream("test.txt");
            System.out.println("-------before fis.close--------");

            // close 是需要拦截 IOException 异常
            // 在此位置关闭存在问题，当出现异常
            // 那么会执行到 catch 语句，以下 fis.close 永远不会执行
            // 这样个对象永远不会得到释放，所以必须提供一种机制
            // 当出现任何问题，都会释放相应的资源(恢复到最初状态)
            // 那么就要使用 finally 语句块
            fis.close();
            System.out.println("-------after fis.close--------");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

- 采用 finally 来释放资源

```java
import java.io.*;

public class ExceptionTest08 {
    public static main(String[] args) {
        // 因为 fis 的作用域问题，必须放到 try 语句块外，局部变量必须给初始值
        // 因为是对象赋值为 null
        FileInputStream fis = null;

        try {
            fis = new FileInputStream("test.txt");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } finally {
            try {
                System.out.println("-------before fis.close--------");
                // 放到 finally 中的语句，程序出现任何问题都会被执行
                // 所以 finally 中一般放置一些需要及时释放的资源
                fis.close();
                System.out.println("-------after fis.close--------");
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

### 深入 finally

```java
public class ExceptionTest09 {
    public static void main(String[] args) {
        int i1 = 100;
        int i2 = 10;
        try {
            int i3 = i1 / i2;
            System.out.println(i3);
            return;
        } catch (ArithmeticException ae) {
            ae.printStackTrace();
        } finally {
            // 会执行 finally
            System.out.println("----------finally---------");
        }
    }
}
```

```java
public class ExceptionTest10 {
    public static void main(String[] args) {
        int i1 = 100;
        int i2 = 10;
        try {
            int i3 = i1 / i2;
            System.out.println(i3);
            // return;
            System.exit(-1); // Java 虚拟机退出
        } catch (ArithmeticException ae) {
            ae.printStackTrace();
        } finally {
            // 只有 Java 虚拟机退出不会执行 finally
            // 其他任何情况下都会执行 finally
            System.out.println("----------finally---------");
        }
    }
}
```

```java
public class ExceptionTest11 {
    public static void main(String[] args) {
        int r = method1();
        // 输出为: 10
        System.out.println(r);
    }

    private static int method1() {
        int a = 10;
        try {
            return a;
        } finally {
            a = 100;
        }
    }
}
```

```java
public class ExceptionTest12 {
    public static void main(String[] args) {
        int r = method1();
        // 输出为: 100
        System.out.println(r);
    }

    private static int method1() {
        int a = 10;
        try {
            a = 50;
        } finally {
            a = 100;
        }
        return a;
    }
}
```

## throws 关键字声明异常

在方法定义处采用 throws 声明异常，如果声明的异常为检查（受控）异常，那么调用方法必须处理此异常。

```java
import java.io.*;

public class ExceptionTest13 {
    public static void main(String[] args) throws Exception { // 可以采用此种方式声明异常，因为 Exception 是两个异常的父类

        // throws FileNotFoundException, IOException {  // 可以在此声明异常，这样就交给 Java 虚拟机处理了，不建议这样使用

        // 分别处理各个异常
        try {
            readFile();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 可以采用如下方式处理异常
        // 因为 Exception 是 FileNotFoundException 和 IOException 的父类
        // 但是一般不建议采用如下方案处理异常，粒度太粗了，异常信息不明确
        try {
            readFile();
        } catch (Exception e) {
            e.printStackTrace();
        }

        readFile();
    }

    private static void readFile() throws FileNotFoundException, IOException {
        // 声明异常，声明后调用者必须处理
        FileInputStream fis = null;
        try {
            fis = new FileInputStream("test.txt");
        } catch (FileNotFoundException e) {
        } finally {
            if (fis != null) {
                fis.close();
            }
        }
    }
}
```

### 声明非受控异常

```java
public class ExceptionTest14 {
    public static void main(String[] args) {
        // 不需要使用 try...catch..，因为声明的是非受控异常
        // method1();
        // 也可以拦截非受控异常
        try {
            method1();
        } catch (ArithmeticException e) {
            e.printStackTrace();
        }
    }
        // 可以声明非受控异常
    private static void method1() throws ArithmeticException {
        int i1 = 100;
        int i2 = 0;

        int i3 = i1 / i2;
        System.out.println(i3);
    }
}
```

### 深入 throws

```java
public class ExceptionTest04 {

    public static void main(String[] args) {
    try {
        m1();
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    }
    //使用 throws 处理异常不是真正的处理异常而是推卸责任
    //谁调用就会抛给谁
    //上面的 m1 方法如果出现了异常，因为采用的是上抛，给了 JVM，JVM 遇到这个异常就会退出 JVM，下面的这个代码就不会执行
    System.out.println("Hello World");
    }

    public static void m1() throws FileNotFoundException {
        m2();
    }

    public static void m2() throws FileNotFoundException {
        m3();
    }

    public static void m3() throws FileNotFoundException {
        new FileInputStream("c:/ab.txt"); //FIleInputStream构造方法声明位置上使用throws（向上抛）
    }
}
```

## 异常的捕获顺序

```java
import java.io.*;

public class ExceptionTest18 {
    public static void main(String[] args) {
        try {
            FileInputStream fis = new FileInputStream("test.txt");
            fis.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        // 将 IOException 放到前面，会出现编译问题
        // 因为 IOException 是 FileNotFoundException 的父类，
        // 所以截获了 IOException 异常后，IOException 的子异常
        // 都不会执行到，所以再次截获 FileNotFoundException 没有任何意义
        // 异常的截获一般按照由小到大的顺序，也就是先截获子异常，再截获父异常
    }
}
```

## 如何自定义异常

自定义异常通常继承于 Exception 或 RuntimeException，到底继承那个应该看具体情况来定。

1. 编译时异常，直接继承 Exception。
2. 运行时异常，直接继承 RuntimeException。

```java
import java.io.*;

public class ExceptionTest19 {
    public static void main(String[] args) {
        try {
            method1(10, 0);
        } catch (MyException e) {
            // 必须拦截,拦截后必须给出处理，如果不给出处理，就属于吃掉了该异常
            // 系统将不给出任何提示，使程序的调试非常困难
            System.out.println(e.getMessage());
        }
    }

    private static void method1(int value1, int value2) throws MyException {
        if (value2 == 0) {
            throw new MyException("除数为 0");
        }
        int value3 = value1 / value2;
        System.out.println(value3);
    }
}

// 自定义受控异常
class MyException extends Exception {
    public MyException() {
        // 调用父类的默认构造函数
        super();
    }

    public MyException(String message) {
        // 手动调用父类的构造方法
        super(message);
    }
}
```

## 方法覆盖与异常

子类方法不能抛出比父类方法更多的异常，但可以抛出父类方法异常的子异常。

```java
import java.io.*;

public class ExceptionTest21 {
    public static void main(String[] args) {
    }
}

interface UserManager {
    public void login(String username, String password) throws UserNotFoundException;
}

class UserNotFoundException extends Exception {
}

class UserManagerImpl1 implements UserManager {
    // 正确
    public void login(String username, String password) throws UserNotFoundException {
    }
}

class UserManagerImpl2 implements UserManager {
    // 不正确，因为 UserManager 接口没有要求抛出 PasswordFailureException 异常
    // 子类异常不能超出父类的异常范围
    public void login(String username, String password) throws UserNotFoundException, PasswordFailureException {
    }
}

class UserManagerImpl3 implements UserManager {
    // 正确，因为 MyException 是 UserNotFoundException 子类
    // MyException 异常没有超出接口的要求
    public void login(String username, String password) throws UserNotFoundException, MyException {
    }
}

class PasswordFailureException extends Exception {
}

class MyException extends UserNotFoundException {
}
```

{% include busuanzi.html %}
