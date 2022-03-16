# java.lang

<https://www.runoob.com/manual/jdk11api/java.base/java/lang/package-summary.html>

Java Object 类是所有类的父类，也就是说 Java 的所有类都继承了 Object，子类可以使用 Object 的所有方法。

## String

method[https://www.w3cschool.cn/java/java-string.html]

## StringBuilder &  StringBuffer

StringBuilder 类在 Java 5 中被提出，它和 StringBuffer 之间的最大不同在于 StringBuilder 的方法不是线程安全的（线程安全就是多线程访问时，采用了加锁机制，当一个线程访问该类的某个数据时，进行保护，其他线程不能进行访问直到该线程读取完，其他线程才可使用。不会出现数据不一致或者数据污染。线程不安全就是不提供数据访问保护，有可能出现多个线程先后更改数据造成所得到的数据是脏数据）。

由于 StringBuilder 相较于 StringBuffer 有速度优势，所以多数情况下建议使用 StringBuilder 类。然而在应用程序要求线程安全的情况下，则必须使用 StringBuffer 类。

method[https://www.w3cschool.cn/java/java-stringbuffer.html]

## Number & Math

method[https://www.runoob.com/java/java-number.html]

逻辑运算符?:

    variable x = (expression) ? value if true : value if false
instanceof运算符:
如果运算符左侧变量所指的对象，是操作符右侧类或接口(class/interface)的一个对象，那么结果为真。

    ( Object reference variable ) instanceof  (class/interface type)

## System

## Thread
