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

java.lang.System
System类包含几个有用的类字段和方法。 它无法实例化。 System类提供的设施包括标准输入，标准输出和错误输出流; 访问外部定义的属性和环境变量; 加载文件和库的方法; 以及用于快速复制阵列的一部分的实用方法。

System.out.printf()
System.out.println(): 作用是用于在控制台上输出字符串信息。
原理分析：
（1）System是java.lang包中的一个类，里面封装了一些系统相关的重要函数与变量。
（2）System类中的所有成员都是静态的，静态属性和静态方法的调用分别是：类名.字段名、类名.方法名。在System类中，out是一个PrintStream类型的静态成员变量，因此可以通过System.out的形式来调用。
在System.class文件中，可以看到声明：public final static PrintStream out = null;
（3）同时，out是一个PrintStream类的对象，PrintStream类有println()方法，因此out对象可以调用println()方法。即out.println()

## Thread


## Exception

所有的异常类是从 java.lang.Exception 类继承的子类。  
Exception 类是 Throwable 类的子类。除了Exception类外，Throwable 还有一个子类 Error 。  
Java 程序通常不捕获错误。错误一般发生在严重故障时，它们在 Java 程序处理的范畴之外。  
Error 用来指示运行时环境发生的错误。

异常类有两个主要的子类：IOException 类和 RuntimeException 类。

### 捕获异常

try/catch 代码块中的代码称为保护代码，使用  try/catch 的语法如下：

    try
    {
    // 程序代码
    }catch(ExceptionName e1)
    {
    //Catch 块
    }

Catch 语句包含要捕获异常类型的声明。当保护代码块中发生一个异常时，try 后面的 catch 块就会被检查。
如果发生的异常包含在 catch 块中，异常会被传递到该 catch 块，这和传递一个参数到方法是一样。

下面的例子中声明有两个元素的一个数组，当代码试图访问数组的第三个元素的时候就会抛出一个异常。

    // 文件名 : ExcepTest.java
    import java.io.*;
    public class ExcepTest{

    public static void main(String args[]){
        try{
            int a[] = new int[2];
            System.out.println("Access element three :" + a[3]);
        }catch(ArrayIndexOutOfBoundsException e){
            System.out.println("Exception thrown  :" + e);
        }
        System.out.println("Out of the block");
    }
    }
以上代码编译运行输出结果如下：

    Exception thrown  :java.lang.ArrayIndexOutOfBoundsException: 3
    Out of the block

### 多重捕获

该实例展示了怎么使用多重 try/catch。

    try
    {
    file = new FileInputStream(fileName);
    x = (byte) file.read();
    }catch(IOException i)
    {
    i.printStackTrace();
    return -1;
    }catch(FileNotFoundException f) //Not valid!
    {
    f.printStackTrace();
    return -1;
    }

### throws/throw关键字

如果一个方法没有捕获一个检查性异常，那么该方法必须使用 throws 关键字来声明。throws 关键字放在方法签名的尾部。

也可以使用 throw 关键字抛出一个异常，无论它是新实例化的还是刚捕获到的。

下面方法的声明抛出一个 RemoteException 异常：

import java.io.*;
public class className
{
   public void deposit(double amount) throws RemoteException
   {
      // Method implementation
      throw new RemoteException();
   }
   //Remainder of class definition
}

### finally 关键字

finally 关键字用来创建在 try 代码块后面执行的代码块。

无论是否发生异常，finally 代码块中的代码总会被执行。

在 finally 代码块中，可以运行清理类型等收尾善后性质的语句。

finally 代码块出现在 catch 代码块最后，语法如下：

    try{
        // 程序代码
    }catch(异常类型1 异常的变量名1){
        // 程序代码
    }catch(异常类型2 异常的变量名2){
        // 程序代码
    }finally{
        // 程序代码
    }

声明自定义异常
在 Java 中你可以自定义异常。编写自己的异常类时需要记住下面的几点。
所有异常都必须是 Throwable 的子类。

如果希望写一个检查性异常类，则需要继承 Exception 类。

如果你想写一个运行时异常类，那么需要继承 RuntimeException 类。

可以像下面这样定义自己的异常类：

        class MyException extends Exception{
        }
实例：

    // 文件名InsufficientFundsException.java
    import java.io.*;

    public class InsufficientFundsException extends Exception
    {
    private double amount;
    public InsufficientFundsException(double amount)
    {
        this.amount = amount;
    } 
    public double getAmount()
    {
        return amount;
    }
    }
为了展示如何使用我们自定义的异常类，

在下面的 CheckingAccount 类中包含一个 withdraw() 方法抛出一个 InsufficientFundsException 异常。