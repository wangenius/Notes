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

static修饰符
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

>无法输出结果 无法从静态环境引用非静态方法.

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
