---
layout: post
title: Java 常用类
description:
image: https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401101901133.jpg
category:
- 编程语言
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
date: 2024-01-11 03:43 +0800
---
## String

### String类是不可变类，也就是说String对象声明后，将不可修改

```java
  String s1 = "a";
  String s2 = "b";
  s1=s1 + s2; //ab
  System.out.println(s1); //new String(“a”);
```

![memory](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401101909230.png)

String对象赋值后不能再修改，这就是不可变对象，如果对字符串修改，那么将会创建新的对象。

> 注意：只要采用双引号赋值字符串，那么在编译期将会放到方法区中的字符串的常量池里,如果是运行时对字符串相加或相减会放到堆中（放之前会先验证方法区中是否含有相同的字符串常量，如果存在，把地址返回，如果不存在，先将字符串常量放到池中，然后再返回该对象的地址）。

### String s1 = “abc”和 String s2 = new String(“abc”)

```java
    String s1 = "abc";
    String s2 = "abc";
    String s3 = new String("abc");
    System.out.println("s1==s2, " + (s1 == s2));
    System.out.println("s2==s3, " + (s2 == s3));
    System.out.println("s2 equlas s3," + (s2.equals(s3)));
```

![memory](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401101915524.png)

- 如果是采用双引号引起来的字符串常量，首先会到常量池中去查找，如果存在就不再分配，如果不存在就分配，常量池中的数据是在编译期赋值的，也就是生成class文件时就把它放到常量池里了，所以s1和s2都指向常量池中的同一个字符串“abc”。

- 关于s3，s3采用的是new的方式，在new的时候存在双引号，所以他会到常量区中查找“abc”，而常量区中存在“abc”，所以常量区中将不再放置字符串，而new关键子会在堆中分配内存，所以在堆中会创建一个对象abc，s3会指向abc。

- 如果比较s2和s3的值必须采用equals，String已经对equals方法进行了覆盖。

### String常用方法简介

1. endsWith：判断字符串是否以指定的后缀结束
2. startsWith: 判断字符串是否以指定的前缀开始
3. equals: 字符串相等比较，不忽略大小写
4. equalsIgnoreCase: 字符串相等比较，忽略大小写
5. indexOf: 取得指定字符在字符串的位置
6. lastIndexOf: 返回最后一次字符串出现的位置
7. length: 取得字符串的长度
8. replaceAll: 替换字符串中指定的内容
9. split: 根据指定的表达式拆分字符串
10. substring: 截子串
11. trim: 去前尾空格
12. valueOf: 将其他类型转换成字符串

### 使用String时的注意事项

因为String是不可变对象，如果多个字符串进行拼接，将会形成多个对象，这样可能会造成内存溢出，会给垃圾回收带来工作量，如下面的应用最好不要用String。

```java
    String s = "";
    for (int i = 0; i < 100; i++) {
      // 以下语句会生成大量的对象
      // 因为String是不可变对象
      // 存在大量的对象相加或相减一般不建议使用String
      // 建议使用StringBuffer或StringBuilder
      s += i; // s = s+i;
    }
```

### StringBuffer和StringBuilder

#### StringBuffer

StringBuffer称为字符串缓冲区，它的工作原理是：预先申请一块内存，存放字符序列，如果字符序列满了，会重新改变缓存区的大小，以容纳更多的字符序列。StringBuffer是可变对象，这个是String最大地不同。

```java
    StringBuffer sbStr = new StringBuffer();
    for (int i = 0; i < 100; i++) {
      sbStr.append(i);
      sbStr.append(",");

      // 方法链的编程风格
      sbStr.append(i).append(",");

      // 拼串去除逗号
      sbStr.append(i);
      if (i != 99) {
      sbStr.append(",");
      }
    }

    // 可以输出
    System.out.println(sbStr);
    System.out.println(sbStr.toString());
    // 去除逗号
    System.out.println(sbStr.toString().substring(0, sbStr.toString().length() - 1));
    System.out.println(sbStr.substring(0, sbStr.length() - 1));
```

#### StringBuilder

用法同StringBuffer，StringBuilder和StringBuffer的区别是StringBuffer中所有的方法都是同步的，是线程安全的，但速度慢，StringBuilder的速度快，但不是线程安全的。

## 基本类型对应的包装类

| 基本类型 |  包装类   |
| :------: | :-------: |
|   byte   |   Byte    |
|  short   |   Short   |
|   char   | Character |
|   long   |   Long    |
|  float   |   Float   |
|  double  |  Double   |
| boolean  |  Boolean  |

### 类的层次结构

![class](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401101929522.png)

除了boolean和Character外，其它的包装类都有valueOf()和parseXXX方法，并且还具有byteVaue(),shortVaue(),intValue(),longValue(),floatValue()和doubleValue()方法，这些方法是最常用的方法。

```java
    int i1 = 100;
    Integer i2 = new Integer(i1);
    double i3 = i2.doubleValue();
    String s = "123";
    int i4 = Integer.parseInt(s);
    Integer i5 = new Integer(s);
    Integer i6 = Integer.valueOf(s);
```

## 日期类

常用日期类：

- java.util.Date
- java.text.SimpleDateFormat
- java.util.Calendar

```java
    // 取得今天的日期
    Date today = new Date();
    System.out.println(today);

    // 格式化日期
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    System.out.println(sdf.format(today));

    Calendar c = Calendar.getInstance();
    System.out.println(c.get(Calendar.DAY_OF_MONTH));
    // 取得2000-10-01为星期几
    Date d = new SimpleDateFormat("yyyy-MM-dd").parse("2000-10-01");
    c.setTime(d);
    System.out.println(c.get(Calendar.DAY_OF_WEEK));
```

## 数字类

### Java.text.DecimalFormat和java.math.BigDecimal

```java
    // 加入千分位，保留两位小数
    DecimalFormat df = new DecimalFormat("###,###.##");
    System.out.println(df.format(1234.23452));

    // 加入千分位保留4位小数，不够补零
    System.out.println(new DecimalFormat("###,###.0000").format(12345.12));

    BigDecimal v1 = new BigDecimal(10);
    BigDecimal v2 = new BigDecimal(20);
    // 相加运算
    BigDecimal v3 = v1.add(v2);
    System.out.println(v3);
```

## Random

Random 位于 java.util 包下，可以产生随机数。

### 生成5个0~100之间的整数随机数

```java
    Random r = new Random();
    for (int i = 0; i < 5; i++) {
      System.out.println(r.nextInt(100));
    }
```

## Enum

### 为什么使用枚举？

```java
// 以下返回1或0存在问题
// 在编译器就容易把程序错了，如：1和111没有什么区别，编译器认为两者是一样的
// 不会报错，错误发现的越早越好，最好在编译器把所有的错误都消除掉

public class Test {
  public static void main(String[] args) throws Exception {
    int ret = method1(10, 2);
    if (ret == 1) {
      System.out.println("成功！");
    }
    if (ret == 0) {
      System.out.println("失败！");
    }
  }

  // 正确返回1，失败返回：0
  private static int method1(int value1, int value2) {
    try {
      int v = value1 / value2;
      return 1;
    } catch (Exception e) {
      return 0;
    }
  }
}
```

```java
// 此种方式比第一种方案好一些
// 有一个统一的约定，成功用1表示，失败采用0标识
// 但是也存在问题，如果不准许约定也会产生问题
// 如果成功我们可以返回SUCCESS,但也可以返回100，因为返回值为int，
// 并没有强制约束要返回1或0

public class Test {
  private static final int SUCCESS = 1;

  private static final int FAILURE = 0;

  public static void main(String[] args) throws Exception {
    int ret = method1(10, 2);
    if (ret == SUCCESS) {
      System.out.println("成功！");
    }
    if (ret == FAILURE) {
      System.out.println("失败！");
    }
  }

  // 正确返回1，失败返回：0
  private static int method1(int value1, int value2) {
    try {
      int v = value1 / value2;
      return SUCCESS;
    } catch (Exception e) {
      return FAILURE;
    }
  }
}
```

### 采用枚举改进

```java
// 使用枚举类型，能够限定取值的范围
// 使程序在编译时就会及早的返现错误
// 这样程序会更加健壮
public class Test {

  public static void main(String[] args) throws Exception {
    Result r = method1(10, 2);
    if (r == Result.SUCCESS) {
      System.out.println("成功！");
    }
    if (r == Result.FAILURE) {
      System.out.println("失败！");
    }
  }

  // 正确返回SUCCESS，失败返回：FAILURE
  private static Result method1(int value1, int value2) {
    try {
      int v = value1 / value2;
      return Result.SUCCESS;
    } catch (Exception e) {
      return Result.FAILURE;
    }
  }
}

enum Result {
  SUCCESS,
  FAILURE
}
```
