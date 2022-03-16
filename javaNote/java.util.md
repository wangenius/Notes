# java.util

## Object

## ArrayList[]

用来存储固定大小的同类型元素。
首先必须声明数组变量，才能在程序中使用数组。
变量声明和实例创建。

    dataType[] arrayRefVar; /声明数组
    arrayRefVar = new dataType[arraySize]; /实例化创建数组
    dataType[] arrayRefVar = new dataType[arraySize]; /数组变量的声明，和创建数组可以用一条语句完成
    dataType[] arrayRefVar = {value0, value1, ..., valuek};

foreach 遍历数组

    for(type element: array){
        System.out.println(element);
    }

Instance:

    public class TestArray {

        public static void main(String[] args) {
            double[] myList = {1.9, 2.9, 3.4, 3.5};

            // 打印所有数组元素
            for (double element: myList) {
                System.out.println(element);
            }
        }
    }

## Calender

Calendar类的功能要比Date类强大很多，而且在实现方式上也比Date类要复杂一些。  
Calendar类是一个抽象类，在实际使用时实现特定的子类的对象，创建对象的过程对程序员来说是透明的，只需要使用getInstance方法创建即可。

    Calendar c = Calendar.getInstance();//默认是当前日期

    //创建一个代表2009年6月12日的Calendar对象
    Calendar c1 = Calendar.getInstance();
    c1.set(2009, 6 - 1, 12);
![avatar](/pic/calendar.png)


## Date

       // 初始化 Date 对象
       Date date = new Date();
        
       // 使用 toString() 函数显示日期时间
       System.out.println(date.toString());

Java使用以下三种方法来比较两个日期：

+ 使用getTime( ) 方法获取两个日期（自1970年1月1日经历的毫秒数值），然后比较这两个值。
+ 使用方法before()，after()和equals()。例如，一个月的12号比18号早，则new Date(99, 2, 12).before(new Date (99, 2, 18))返回true。
+ 使用compareTo()方法，它是由Comparable接口定义的，Date类实现了这个接口。

SimpleDateFormat 格式化

java.text包 下的一个类 SimpleDateFormat是一个具体的类，用于以区域设置敏感的方式格式化和解析日期。

    import java.util.*;
    import java.text.*;

    public class DateDemo {
        public static void main(String args[]) {

            Date dNow = new Date( );
            SimpleDateFormat ft = 
            new SimpleDateFormat ("E yyyy.MM.dd 'at' hh:mm:ss a zzz");

            System.out.println("Current Date: " + ft.format(dNow));
        }
    }

SimpleDateFormat.parse():

    import java.util.*;
    import java.text.*;
    
    public class DateDemo {

    public static void main(String args[]) {
        SimpleDateFormat ft = new SimpleDateFormat ("yyyy-MM-dd"); 

        String input = args.length == 0 ? "1818-11-11" : args[0]; 

        System.out.print(input + " Parses as "); 

        Date t; 

        try { 
            t = ft.parse(input); 
            System.out.println(t); 
        } catch (ParseException e) { 
            System.out.println("Unparseable using " + ft); 
        }
    }
    }

|  letter   | describe  | instance|
|  ----  | ----  |----|
|y|四位年份|2001|
|M|月份|July or 07|
|d|一个月的日期|10|
|h|12小时⏲|2|
|H|24小时⏲|14|
|m|分钟数|30|
|s|秒数|55|
|E|星期几|Tuesday|

printf格式化

    import java.util.Date;

    public class DateDemo {

    public static void main(String args[]) {
        // 初始化 Date 对象
        Date date = new Date();

        // 使用toString()显示日期和时间
        String str = String.format("Current Date/Time : %tc", date );

        System.out.printf(str);
    }
    }

Java 休眠(sleep)
你可以让程序休眠一毫秒的时间或者到您的计算机的寿命长的任意段时间。

import java.util.*;
  
    public class SleepDemo {
        public static void main(String args[]) {
            try { 
                System.out.println(new Date( ) + "\n"); 
                Thread.sleep(5*60*10); 
                System.out.println(new Date( ) + "\n"); 
            } catch (Exception e) { 
                System.out.println("Got an exception!"); 
            }
        }
    }

## HashMap

## Scanner

## Random