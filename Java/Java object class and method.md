# Java 对象和类和方法

程序设计是通过对象对程序进行设计，对象代表一个实体，实体可以清楚地被识别。

java支持：

1. 多态
2. 继承
3. 封装
4. 抽象 不能实例化对象的类
5. 类
6. 对象 类的实例化
7. 实例
8. 方法 类和对象

对象：对象是类的一个实例，有状态和行为。例如，一条狗是一个对象，它的状态有：颜色、名字、品种；行为有：摇尾巴、叫、吃等。
类：类是一个**模板**，它描述一类对象的行为和状态。

## 静态成员和动态成员

### static修饰符

静态变量：
static 关键字用来声明独立于对象的静态变量，无论一个类实例化多少对象，它的静态变量只有一份拷贝。静态变量也被称为类变量。局部变量不能被声明为static变量。

static 关键字用来声明独立于对象的静态方法。静态方法不能使用类的非静态变量。静态方法从参数列表得到数据，然后计算这些数据。

首先，两者本质上的区别是：静态方法是在类中使用staitc修饰的方法，在类定义的时候已经被装载和分配。而非静态方法是不加static关键字的方法，在类定义时没有占用内存，只有在类被实例化成对象时，对象调用该方法才被分配内存。

其次，静态方法中只能调用静态成员或者方法，不能调用非静态方法或者非静态成员，而非静态方法既可以调用静态成员或者方法又可以调用其他的非静态成员或者方法。

    class Test{
        public int sum(int a,int b){//非静态方法
        return a+b;
        }
        public static void main(String[] args){
            int  result=sum(1,2);//静态方法调用非静态方法
            System.out.println("result="+result);
        }
    }

*无法输出结果 无法从静态环境引用非静态方法.*

解决方法：1静态方法只能访问静态方法和静态成员。

    class Test{
        public static int sum(int a,int b){//加入static关键字，变成静态方法
        return a+b;
        }
        public static void main(String[] args){
            int  result=sum(1,2);//静态方法调用静态方法
            System.out.println("result="+result);
        }
    }
2 非静态方法需要实例化

    class Test{
        public int sum(int a,int b){
        return a+b;
        }
        public static void main(String[] args){
            Test test=new Test();//实例化类
            int  result=test.sum(1,2);//调用非静态方法
            System.out.println("result="+result);
        }
    }

案例：

    public class Test extends Base{
    
        static{
            System.out.println("test static");
        }
        
        public Test(){
            System.out.println("test constructor");
        }
        
        public static void main(String[] args) {
            new Test();
        }
    }
    
    class Base{
        
        static{
            System.out.println("base static");
        }
        
        public Base(){
            System.out.println("base constructor");
        }
    }

>输出：

    base static
    test static
    base constructor
    test constructor

*这段代码具体的执行过程，在执行开始，先要寻找到main方法，因为main方法是程序的入口，但是在执行main方法之前，必须先加载Test类，而在加载Test类的时候发现Test类继承自Base类，因此会转去先加载Base类，在加载Base类的时候，发现有static块，便执行了static块。在Base类加载完成之后，便继续加载Test类，然后发现Test类中也有static块，便执行static块。在加载完所需的类之后，便开始执行main方法。在main方法中执行new Test()的时候会先调用父类的构造器，然后再调用自身的构造器。*

    public class Test {
        Person person = new Person("Test");
        static{
            System.out.println("test static");
        }
        
        public Test() {
            System.out.println("test constructor");
        }
        
        public static void main(String[] args) {
            new MyClass();  /生成对象（加载类——构造器方法）
        }
    }
    
    class Person{
        static{
            System.out.println("person static");
        }
        public Person(String str) {
            System.out.println("person "+str);
        }
    }
    
    
    class MyClass extends Test {
        Person person = new Person("MyClass");
        static{
            System.out.println("myclass static");
        }
        
        public MyClass() {
            System.out.println("myclass constructor");
        }
    }
output:

    test static
    myclass static
    person static
    person Test
    test constructor
    person MyClass
    myclass constructor

*首先加载Test类，因此会执行Test类中的static块。接着执行new MyClass()，而MyClass类还没有被加载，因此需要加载MyClass类。在加载MyClass类的时候，发现MyClass类继承自Test类，但是由于Test类已经被加载了，所以只需要加载MyClass类，那么就会执行MyClass类的中的static块。在加载完之后，就通过构造器来生成对象。而在生成对象的时候，必须先初始化父类的成员变量，因此会执行Test中的Person person = new Person()，而Person类还没有被加载过，因此会先加载Person类并执行Person类中的static块，接着执行父类的构造器，完成了父类的初始化，然后就来初始化自身了，因此会接着执行MyClass中的Person person = new Person()，最后执行MyClass的构造器。*

1. 执行类中的static块
2. 执行main方法
3. 实例化时先加载类 类只加载一次

>加载类——执行静态块——执行main方法——生成对象时先执行成员变量初始化（需要从父级开始）——执行构造器方法

### final 修饰符

final 变量能被显式地初始化并且只能初始化一次。被声明为final的对象的引用不能指向不同的对象。但是 final 对象里的数据可以被改变。也就是说 final 对象的引用不能改变，但是里面的值可以改变。

final 修饰符通常和 static 修饰符一起使用来创建类常量。

类中的 Final 方法可以被子类继承，但是不能被子类修改。声明 final 方法的主要目的是防止该方法的内容被修改。

final 类不能被继承，没有类能够继承 final 类的任何特性。

### abstract 修饰符

抽象类：
抽象类不能用来实例化对象，声明抽象类的唯一目的是为了将来对该类进行扩充。

一个类不能同时被 abstract 和 final 修饰。如果一个类包含抽象方法，那么该类一定要声明为抽象类，否则将出现编译错误。

抽象类可以包含抽象方法和非抽象方法。

所有的对象都是通过类来描绘的，但是反过来，并不是所有的类都是用来描绘对象的，如果一个类中没有包含足够的信息来描绘一个具体的对象，这样的类就是抽象类。

GeometricObject就成为一个抽象类。
抽象方法指一些只有方法声明，而没有具体方法体的方法。
在方法头中使用 abstract 修饰符表示

>final class无子类 final method可继承不可修改  
abstract class无实例 需继承 abstract方法需要继承

### 方法

    修饰符 返回值类型 方法名 (参数类型 参数名){
        ...
        方法体
        ...
        return 返回值;
    }
>方法包含一个方法头和一个方法体。下面是一个方法的所有部分：  
修饰符：修饰符，这是可选的，告诉编译器如何调用该方法。定义了该方法的访问类型。  
返回值类型 ：方法可能会返回值。returnValueType是方法返回值的数据类型。有些方法执行所需的操作，但没有返回值。在这种情况下，returnValueType是关键字void。  
方法名：是方法的实际名称。方法名和参数表共同构成方法签名。  
参数类型：参数像是一个占位符。当方法被调用时，传递值给参数。这个值被称为实参或变量。参数列表是指方法的参数类型、顺序和参数的个数。参数是可选的，方法可以不包含任何参数。  
方法体：方法体包含具体的语句，定义该方法的功能。

#### 重载

一个类的两个方法拥有相同的名字，但是有不同的参数列表。  
Java编译器根据方法签名判断哪个方法应该被调用。  
方法重载可以让程序更清晰易读。执行密切相关任务的方法应该使用相同的名字。 

#### 作用域

![scope](https://atts.w3cschool.cn/attachments/image/20160401/1459506313251902.jpg)

#### 构造方法

constructor
当一个对象被创建时候，构造方法用来初始化该对象。构造方法和它所在类的名字相同，但构造方法没有返回值。  
通常会使用构造方法给一个类的实例变量赋初值，或者执行其它必要的步骤来创建一个完整的对象。  
不管你是否自定义构造方法，所有的类都有构造方法，因为Java自动提供了一个默认构造方法，它把所有成员初始化为0。  
一旦你定义了自己的构造方法，默认构造方法就会失效。

#### finalize()

### 继承

Java 只支持单继承，也就是说，一个类不能继承多个类。

下面的做法是不合法的：

public class extends Animal, Mammal{} 
Java 只支持单继承（继承基本类和抽象类），但是我们可以用接口来实现（多继承接口来实现），代码结构如下：

public class Apple extends Fruit implements Fruit1, Fruit2{}
一般我们继承基本类和抽象类用 extends 关键字，实现接口类的继承用 implements 关键字。

### override 重写

重写是子类对父类的允许访问的方法的实现过程进行重新编写！返回值和形参都不能改变。即外壳不变，核心重写！

重写的好处在于子类可以根据需要，定义特定于自己的行为。

也就是说子类能够根据需要实现父类的方法。

在面向对象原则里，重写意味着可以重写任何现有方法。

    class Animal{

    public void move(){
        System.out.println("动物可以移动");
    }
    }

    class Dog extends Animal{

    public void move(){
        System.out.println("狗可以跑和走");
    }
    }

    public class TestDog{

    public static void main(String args[]){
        Animal a = new Animal(); // Animal 对象
        Animal b = new Dog(); // Dog 对象

        a.move();// 执行 Animal 类的方法

        b.move();//执行 Dog 类的方法
    }
    }

以上实例编译运行结果如下：

    动物可以移动
    狗可以跑和走

Super 关键字的使用
当需要在子类中调用父类的被重写方法时，要使用 super 关键字。

    class Animal{

    public void move(){
        System.out.println("动物可以移动");
    }
    }

    class Dog extends Animal{

    public void move(){
        super.move(); // 应用super类的方法
        System.out.println("狗可以跑和走");
    }
    }

    public class TestDog{

    public static void main(String args[]){

        Animal b = new Dog(); //
        b.move(); //执行 Dog类的方法

    }
    }
以上实例编译运行结果如下：

    动物可以移动
    狗可以跑和走

### 重载 overload

    public class Overloading {
    
        public int test(){
            System.out.println("test1");
            return 1;
        }
    
        public void test(int a){
            System.out.println("test2");
        }
    
        //以下两个参数类型顺序不同
        public String test(int a,String s){
            System.out.println("test3");
            return "returntest3";
        }
    
        public String test(String s,int a){
            System.out.println("test4");
            return "returntest4";
        }
    
        public static void main(String[] args){
            Overloading o = new Overloading();
            System.out.println(o.test());
            o.test(1);
            System.out.println(o.test(1,"test3"));
            System.out.println(o.test("test4",1));
        }
    }
以上实例编译运行结果如下：

    test1
    1
    test2
    test3
    returntest3
    test4
    returntest4

### Java 多态

多态是同一个行为具有多个不同表现形式或形态的能力。

多态性是对象多种表现形式的体现。

    public interface Vegetarian{}
    public class Animal{}
    public class Deer extends Animal implements Vegetarian{}
    //因为Deer类具有多重继承，所以它具有多态性。以上实例解析如下：

    一个 Deer IS-A（是一个） Animal
    一个 Deer IS-A（是一个） Vegetarian
    一个 Deer IS-A（是一个） Deer
    一个 Deer IS-A（是一个）Object
在Java中，所有的对象都具有多态性，因为任何对象都能通过IS-A测试的类型和Object类。

虚方法
重写 引用

多态的实现方式

方式一：重写：
这个内容已经在上一章节详细讲过，就不再阐述，详细可访问：Java 重写(Override)与重载(Overload)。

方式二：接口

1. 生活中的接口最具代表性的就是插座，例如一个三接头的插头都能接在三孔插座中，因为这个是每个国家都有各自规定的接口规则，有可能到国外就不行，那是因为国外自己定义的接口类型。
2. java中的接口类似于生活中的接口，就是一些方法特征的集合，但没有方法的实现。具体可以看 java接口 这一章节的内容。

方式三：抽象类和抽象方法

### Encapsulation 封装

一种将抽象性函式接口的实作细节部份包装、隐藏起来的方法。  
封装可以被认为是一个保护屏障，防止该类的代码和数据被外部类定义的代码随机访问。  
要访问该类的代码和数据，必须通过严格的接口控制。  
封装最主要的功能在于我们能修改自己的实现代码，而不用修改那些调用我们代码的程序片段。  
适当的封装可以让程式码更容易理解与维护，也加强了程式码的安全性。

### Interface 接口

在JAVA编程语言中是一个抽象类型，是抽象方法的集合，接口通常以interface来声明。一个类通过继承接口的方式，从而来继承接口的抽象方法。

接口并不是类，编写接口的方式和类很相似，但是它们属于不同的概念。类描述对象的属性和方法。接口则包含类要实现的方法。

除非实现接口的类是抽象类，否则该类要定义接口中的所有方法。

接口无法被实例化，但是可以被实现。一个实现接口的类，必须实现接口内所描述的所有方法，否则就必须声明为抽象类。另外，在Java中，接口类型可用来声明一个变量，他们可以成为一个空指针，或是被绑定在一个以此接口实现的对象。

接口与类相似点：

一个接口可以有多个方法。
接口文件保存在.java结尾的文件中，文件名使用接口名。
接口的字节码文件保存在.class结尾的文件中。
接口相应的字节码文件必须在与包名称相匹配的目录结构中。
接口与类的区别：

接口不能用于实例化对象。
接口没有构造方法。
接口中所有的方法必须是抽象方法。
接口不能包含成员变量，除了static和final变量。
接口不是被类继承了，而是要被类实现。
接口支持多重继承。

Interface关键字用来声明一个接口。下面是接口声明的一个简单例子。

    /* 文件名 : NameOfInterface.java */
    import java.lang.*;
    //引入包

    public interface NameOfInterface
    {
    //任何类型 final, static 字段
    //抽象方法
    }
接口有以下特性：

接口是隐式抽象的，当声明一个接口的时候，不必使用abstract关键字。
接口中每一个方法也是隐式抽象的，声明时同样不需要abstract关键子。
接口中的方法都是公有的。

当类实现接口的时候，类要实现接口中所有的方法。否则，类必须声明为抽象的类。

类使用implements关键字实现接口。在类声明中，Implements关键字放在class声明后面。

    /* 文件名 : Animal.java */
        interface Animal {

        public void eat();
        public void travel();
    }

实现一个接口的语法，可以使用这个公式：

... implements 接口名称[, 其他接口, 其他接口..., ...] ...
实例

    /* 文件名 : MammalInt.java */
    public class MammalInt implements Animal{

    public void eat(){
        System.out.println("Mammal eats");
    }

    public void travel(){
        System.out.println("Mammal travels");
    } 

    public int noOfLegs(){
        return 0;
    }

    public static void main(String args[]){
        MammalInt m = new MammalInt();
        m.eat();
        m.travel();
    }
    } 
    //以上实例编译运行结果如下:

    Mammal eats
    Mammal travels

一个接口能继承另一个接口，和类之间的继承方式比较相似。接口的继承使用extends关键字，子接口继承父接口的方法。
在Java中，类的多重继承是不合法，但接口允许多重继承，。

在接口的多重继承中extends关键字只需要使用一次，在其后跟着继承接口。

在Java中，类的多重继承是不合法，但接口允许多重继承，。

在接口的多重继承中extends关键字只需要使用一次，在其后跟着继承接口。 如下所示：

public interface Hockey extends Sports, Event
以上的程序片段是合法定义的子接口，与类不同的是，接口允许多重继承，而 Sports及 Event 可能定义或是继承相同的方法