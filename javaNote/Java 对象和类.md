# Java 对象和类

程序设计是通过对象对程序进行设计，对象代表一个实体，实体可以清楚地被识别。

java支持：

1. 多态
2. 继承
3. 封装
4. 抽象
5. 类
6. 对象
7. 实例
8. 方法

对象：对象是类的一个实例，有状态和行为。例如，一条狗是一个对象，它的状态有：颜色、名字、品种；行为有：摇尾巴、叫、吃等。
类：类是一个**模板**，它描述一类对象的行为和状态。

## 静态方法和动态方法

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
