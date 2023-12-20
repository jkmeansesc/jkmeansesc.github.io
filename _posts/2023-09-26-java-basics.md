---
layout: post
title: Java 语言基础
date: 2023-09-26 04:33 +0800
description:
image: https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-09-28-1695871254.jpg
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
lang: zh-CN
---

## Java 的发展历程

Java 来自 sun，aka 斯坦福大学网络。

|     时间     | 描述                                                                                                          |
| :----------: | :------------------------------------------------------------------------------------------------------------ |
| 1991 ～ 1995 | Sun 为了占领智能费电子产品市场， 由 james gosling 负责该项目，来开发 Oak 语言                                 |
|     1995     | 将 Oak 改名为 Java                                                                                            |
|     1996     | JDK1.0                                                                                                        |
|     1997     | JDK1.1                                                                                                        |
|     1998     | JDK1.2，将该版本命名为 J2SDK,将 Java 更名为 Java 2                                                            |
|     1999     | 将 java 分为三大块:J2SE(Java 标准版)、J2EE (Java 企业版)、J2ME(Java 微型版本)                                 |
|     2000     | J2SE1.3                                                                                                       |
|     2002     | J2SE1.4                                                                                                       |
|     2004     | 此时不再叫 J2SE1.5,叫 5.0                                                                                     |
|     2005     | 2005 Java 10 周年，将 J2SE 改为 JAVASE、 将 J2EE 改为 JAVAEE、将 J2ME 改为 JAVA                               |
|     2006     | JDK6                                                                                                          |
|     2009     | Oracle 收购 SUN 公司，BEA 公司（JavaEE 服务器 Weblogic）。                                                    |
|     2011     | JDK7 发布，提供新的 G1 收集器、加强对非 Java 语言的调用支持、可并行的类加载架构等。                           |
|     2014     | JDK8                                                                                                          |
|     2017     | JDK9                                                                                                          |
|     2018     | JDK10，主要对内部进行重构，统一源仓库，统一垃圾收集器接口，统一即时编译器接口                                 |
|     2018     | JDK11，这也是一个 LTS（long-term support）版本，包含 17 个 JEP，同时被引入的还有 ZGC 这样革命性的垃圾收集器。 |
|     2019     | JDK12，RedHat 接手了 OpenJDK 8 和 OpenJDk 11 的管理和维护权。                                                 |
|     2019     | JDK13                                                                                                         |
|     2020     | JDK14                                                                                                         |
|     2020     | JDK15                                                                                                         |
|     2021     | JDK16                                                                                                         |
|     2021     | JDK17，JDK 11 之后的下一个 LTS 版本。                                                                         |

## Identifier - 标识符

标识符可以标识类名，接口名，变量名，方法名

- 由数字，字母，下划线和美元符号组成，其他不行
- 不能以数字开头
- 关键字不能作为标识符
- 标识符区分大小写
- 标识符理论上没有长度限制

| 合法标识符 | 不合法标识符 |
| :--------: | :----------: |
| \_123Test  |   123Test    |
| HelloWorld | Hello-World  |
| HelloWorld | HelloWorld#  |
|  public1   |    public    |
| HelloWord  | Hello World  |

### Camel Case - 驼峰命名法

- 类名称的第一个字符大写，接下来遇到新词首字母大写
- 方法名称首字母小写，接下来遇到新词首字母大写
- 变量也是首字母小写，接下来遇到新词首字母大写

## Keywords - 关键字

| Keywords |              |          |            |
| :------: | :----------: | :------: | :--------: |
| abstract |    assert    | boolean  |   break    |
|   byte   |     case     |  catch   |    char    |
|  class   |    const     | continue |  default   |
|    do    |    double    |   else   |    enum    |
| extends  |    final     | finally  |   float    |
|   for    |     goto     |    if    | implements |
|  import  |  instanceof  |   int    | interface  |
|   long   |    native    |   new    |  package   |
| private  |  protected   |  public  |   return   |
|  short   |    static    | strictfp |   super    |
|  switch  | synchronized |   this   |   throw    |
|  throws  |  transient   |   try    |    void    |
| volatile |    while     |          |            |

## Variables - 变量

变量是 java 中的一个最基本的单元，也就是内存中的一块区域。

四个基本属性：

- 变量名: 合法的标识符
- 变量的数据类型: 可以是基本类型和引用类型(必须包含类型)
- 存储单元: 存储单元大小是由数据类型决定的，如:int 为 4 个字节 32 位
- 变量值: 在存储单元中放的就是变量值(如果是基本类型放的就是具体值，如果是引用类型放的是内存地址，如果 null，表示不指向任何对象)

## Type - Java 数据类型

在计算机内部，所有信息都采用二进制表示，每个二进制由 0 和 1 两种状态，一个字节有 8 位，也就是由 8 个 0 或 1 构成。

`short` 类型的 6 在计算机中是如何存储的？short 是两个字节，那么 short 6 的二进制为: 00000000 00000110

`int` 类型的 6 在计算机中存储为 32 位: 00000000 00000000 00000000 00000110

`char` 类型可以存放一个汉字，java 中的 char 使用 utf-16 编码。

```java
        char c = '中';
        System.out.println(c); // 中
```

### 基本类型

|   数据类型   |    关键字     | 内存占用 |   取值范围    |                |
| :----------: | :-----------: | :------: | :-----------: | :------------: |
|    字节型    |     byte      | 1 个字节 |     -128      |      127       |
|    短整型    |     short     | 2 个字节 |    -32768     |     32767      |
|     整型     | int (default) | 4 个字节 | -2 的 31 次方 | 2 的 31 次方-1 |
|    长整型    |     long      | 8 个字节 | -2 的 63 次方 | 2 的 63 次方-1 |
| 单精度浮点数 |     float     | 4 个字节 |  1.4013E-45   |   3.4028E+38   |
| 双精度浮点数 |    double     | 8 个字节 |   4.9E-324    |  1.7977E+308   |
|    字符型    |     char      | 2 个字节 |       0       |     65535      |
|   布尔类型   |    boolean    | 1 个字节 |     TRUE      |     FALSE      |

### 引用数据类型

- 数组
- 类
- 接口

### Literals - 字面值

1. 什么是字面值？

   一眼看上去就知道是多少的数据，就是字面值。

2. 字面值本质：
   - 字面值是有数据类型的：
     - 整型 100
     - 浮点型 3.14
     - 布尔型 true/false
     - 字符型 '中'
     - 字符串型 "ABC"
     - 在内存中占用空间
     - 字面值就是内存中的一块空间，这块空间有类型，有值
     - 只有字面值内存无法得到重复利用
   - java 语言中所有的字符都采用单引号括起来
   - java 语言中所有的字符串都采用双引号括起来

### 基本类型的转换

- 在 java 中基本类型可以相互转换，boolean 类型比较特殊不可以转换成其他类型
- 转换分为默认转换和强制转换:
  - 默认转换:容量小的类型会默认转换为容量大的类型
    - byte-->short/char-->int-->long-->float-->double
    - byte、short、char 之间计算不会互相转换，首先先转换成 int
  - 强制转换:
    - 将容量大的类型转换成容量小的类型，需要进行强制转换
    - 只要不超出范围可以将整型值直接赋值给 byte，short，char
    - 在多种类型混合运算过程中，首先先将所有数据转换成容量最大的那种，再运算

```java
  //出现错误，1000超出了byte的范围
  byte a = 1000;

  //正确，因为20没有超出byte范围
  byte a = 20;

  //正确，因为数值1000没有超出short类型的范围
  short b = 1000;

  //正确，因为默认就是int，并且没有超出int范围
  int c = 1000;

  //正确，可以自动转换
  long d = c;

  //错误，出现精度丢失问题，大类型-->>小类型会出现问题
  int e = d;

  //将long强制转换成int类型
  //因为值1000，没有超出int范围，所以转换是正确的
  int e = (int)d;

  //因为java中的运算会会转成最大类型
  //而10和3默认为int,所以运算后的最大类型也是int
  int f = 10/3;

  //声明10为long类型
  long g = 10;

  //出现错误，多个数值在运算过程中，会转换成容量最大的类型
  //以下示例最大的类型为double，而h为int，所以就会出现大类型(long)到小类型(int)的转换，将会出现精度丢失问题
  int h = g/3;

  //可以强制转换,因为运算结果没有超出int范围
  int h = (int)g/3;

  //可以采用long类型来接收运算结果
  long h = g/3;

  //出现精度损失问题，以下问题主要是优先级的问题
  //将g转换成int，然后又将int类型的g转换成byte,最后byte类型的g和3运算，那么它的运算结果类型就是int，所以int赋值给byte就出现了精度损失问题
  byte h = (byte)(int)g/3;
  //正确
  byte h = (byte)(int)(g/3);
  //不能转换,还有因为优先级的问题
  byte h = (byte)g/3;
  //可以转换，因为运算结果没有超出byte范围
  byte h = (byte)(g/3);
  //可以转换，因为运算结果没有超出short范围
  short h = (short)(g/3);

  short i = 10; byte j = 5;
  //错误，short和byte运算，首先会转换成int再运算
  //所以运算结果为int，int赋值给short就会出现精度丢失问题
  short k = i + j;

  //可以将运算结果强制转换成short
  short k = (short)(i + j);

  //因为运算结果为int，所以可以采用int类型接收
  int k =i + j;

  char l = 'a';
  System. out.println(l);

  //输出结果为 97，也就是a的 ASCII 值
  System.out.println((byte)l);

  int m = l + 100;
  //输出结构为197,取得a的ASCII码值，让后与100进行相加运算
  System.out.println(m);
```

## Operator - 运算符

### 算术运算符

| 运算符  |            运算            |
| :-----: | :------------------------: |
|    +    |          加法运算          |
|    -    |          减法运算          |
|   \*    |          乘法运算          |
|    /    |          除法运算          |
|    %    | 取模运算，两数字相除取余数 |
| ++，- - |        自增自减运算        |

`i++` : 先赋值再运算

```java
  int a = 1;
  int b = a++;

  // ++在变量的后面，先把值赋值给b，然后a再加(也就是先赋值再自加)，所以就输出了a=2 b=1
  System.out.println("a=" + a); // 2
  System.out.println("b=" + b); // 1
```

`++i` : 先运算再赋值

```java
  int a = 1;
  int b = ++a;

  // 输出结果为a=2 b=2，如果++在变量的前面，是先自加在赋值
  System.out.println("a=" + a); // 2
  System.out.println("b=" + b); // 2
```

### 比较运算符

| 运算符 |   运算   |
| :----: | :------: |
|   >    |   大于   |
|   <    |   小于   |
|   >=   | 大于等于 |
|   <=   | 小于等于 |
|   ==   |   等于   |
|   !=   |  不等于  |

### 逻辑运算符

| 运算符 | 运算                                                     |
| :----: | :------------------------------------------------------- |
|   !    | 取反                                                     |
|   &    | 非简洁与，A,B 都为真，结果为真，有一个为假，结果为假     |
|   \|   | 非简洁或，A,B 有一个为真，结果为真，都为假，结果为假     |
|   ^    | 异或，A,B 不同真假时结果为真，同真同假，结果为假         |
|   &&   | 简洁与，A,B 都为真，结果为真，A 为假，则不再判断直接为假 |
|  \|\|  | 简洁或，A,B 都为假，结果为假，A 为真，则不再判断直接为真 |

### 赋值运算符

| 运算符 |  运算  |
| :----: | :----: |
|   =    |  赋值  |
|   +=   | 加等于 |
|   -=   | 减等于 |
|  \*=   | 乘等于 |
|   /=   | 除等于 |
|   %=   | 取模等 |

### 三元运算符

| 运算符 | 运算                                               |
| :----: | :------------------------------------------------- |
| x?a:b  | 判断 x，x 为真，表达式值为 a，x 为假，表达式值为 b |

```java
  int a = 11;
  int b = a > 0 ? 1 : -1;
  System.out.println(b); // 1
  boolean c = a % 2 == 0 ? true : false;
  System.out.println(c); // false
```

## Java 的加载与运行

### 编译时

`.java`源代码 -> Java 编译器 -> `.class`字节码

### 运行时

类加载器 -> 字节码校验器 -> 解释器 / JIT 代码生成器 -> 硬件

## public class 和 class 的区别

- 一个.java 源文件中可以定义多个 class
- 并且一个 class 会生成一个.class 文件
- 采用 public class 来声明 class，那么文件名必须和类名完全一致(包括大小写)
- 如果要定义 public 的 class，那么这个 public 的 class 也只能有一个

## Conditional Statements - 控制语句

Java 控制语句可以分为 7 种:

### 控制条件结构语句

- if、if else

```java
public class test {
    public static void main(String[] args) {

        int age = 3;

        if (age > 0 && age <= 5) {
            System.out.println("幼儿");
        } else if (age > 5 && age <= 10) {
            System.out.println("儿童");
        } else if (age > 10 && age <= 18) {
            System.out.println("少年");
        } else {
            System.out.println("青年");
        }
    }
}
```

- switch

{: .prompt-info }

> 表达式的值只能为:char、byte、short、int 类型，boolean、long、float、double 都是非法的。
> break 语句可以省略，但会出现 switch 穿透。

```java
public class test {
    public static void main(String[] args) {

        char c = 'd';
        switch (c) {
            case 'a':
                System.out.println("优秀");
                break;//注意 break case 'b':
            System.out.println("良好");
            break;
            case 'c':
                System.out.println("一般");
                break;
            default:
                System.out.println("很差");
        }
        System.out.println("switch 执行结束!");
    }
}
```

### 控制循环结构语句

- for

```java
        for (int i = 1; i <= 10; i++) {
            System.out.println(i);
        }
```

- while

```java
        int i = 1;
        //注意死循环问题
        while (i <= 10) {
            System.out.println(i);
            i++;
        }
```

- do while

{: .prompt-info }

> do while 与 while 非常相似，不同点在于 do while 先执行循环体，也就是说不管条件符不符合，循环体至少执行一次

```java
        int i = 1;
        do {
            System.out.println(i);
            i++;
        } while (i <= 10); //注意分号
```

### 改变控制语句顺序

- break

```java
    public static void main(String[] args) {
        for (int i = 1; i <= 5; i++) {
            System.out.println("i===================" + i);
            for (int j = 1; j <= 10; j++) {
                System.out.println(j);
                if (j == 5) {
                    break;
                }
            } //以上 break 会到此为止
        } //以上 break 不会跳到这里
    }
```

- continue

{: .prompt-info }

> continue 只能用在循环语句中，表示在循环中执行到 continue 时，自动结束本次循环，然后判断条件，决定是否进行下一次循环。

```java
        for (int i = 1; i <= 100; i++) {

            if (i % 2 != 0) {
                System.out.println(i);
            }
            if (i % 2 == 0) {
                continue; //继续下一次循环 }
                System.out.println(i);
            }
        }
```

## Method - 方法

### 什么是方法？

- 方法是可以重复调用的代码块。
- 方法在调用的时候，才会在内存分配空间。
- 分配空间指在栈中分配空间（JVM 在内存中有一块内存是栈（stack）内存）。
- 方法调用叫压栈。
- 方法结束叫弹栈。

**语法：**

[方法修饰列表] 返回值类型 方法名(方法参数列表){ 方法体 }

- 方法修饰列表 - Modifier

  可选，方法的修饰符可以包括:

  `public` `protected` `private` `abstract` `static` `final` `synchronized`

  其中 public protected private 不能同时存在。

- 返回值类型 - Return Type

  如果没有返回值使用 void 关键字。

  如果存在返回值可以是基本类型和引用类型， 如果存在返回值，使用 return 语句。

  Return 语句后面不能再执行语句。

- 方法名 - Method Name
  任意合法的标识符

- 方法参数列表 - Parameters/Arguments

  参数列表可以多个，如 method(int a, int b)

```java
    public static void main(String[] args) {
        String s = method1(1);
        System.out.println(s);
    }

    public static String method1(int c) {
        return switch (c) {
            case 1 -> {
                System.out.println("优秀");
                yield "优";
            }
            case 2 -> {
                System.out.println("良好");
                yield "良好";
            }
            case 3 -> {
                System.out.println("一般");
                yield "一般";
            }
            default -> {
                System.out.println("很差");
                yield "很差";
            }
        };
    }
```

### Overload - 方法重载

#### 重载的条件

- 方法名相同。
- 方法的参数类型，个数，顺序至少有一个不同。
- 方法的返回类型可以不同(不依靠返回类型来区分重载)。
- 方法的修饰符可以不同，因为方法重载和修饰符没有任何关系。
- 方法重载只出现在同一个类中。

```java
    public static void main(String[] args) {

        int retInt = sum(10, 20);
        System.out.println(retInt);
        float retFloat = sum(1.5f, 2.5f);
        System.out.println(retFloat);
        double retDouble = sum(2.2, 3.2);
        System.out.println(retDouble);

    }

    //对int求和
    public static int sum(int v1, int v2) {
        return v1 + v2;
    }

    //对float求和
    public static float sum(float v1, float v2) {
        return v1 + v2;
    }

    //对double求和
    public static double sum(double v1, double v2) {
        return v1 + v2;
    }
```

### 方法递归

递归:指方法调用自身。

```java
    public static void main(String[] args) {
        int retValue = method1(5);
        System.out.println(retValue);
    }

    //采用递归求和
    public static int method1(int n) {
        if (n == 1) {
            return 1;
        } else { //递归调用，调用自身
            return n + method1(n - 1);
        }
    }
```

{% include busuanzi.html %}
