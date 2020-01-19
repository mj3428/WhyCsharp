# 深入理解类
## 静态字段
静态字段被类的所有实例共享，所有实例都访问同一内存位置。  
如:
```
class D
{
  int Mem1;
  static int Mem2;
  ...
}

static void Main()
{
  D d1 = new D();
  D d2 = new D();
  ... 
}
```
这里d1和d2都可以有自己的Mem1，但是Mem2的值是共享的。  

## 从类的外部访问静态成员
一般地，访问静态类成员会加上类名`D.Mem2 = 5;`,不过也有另一种方法访问静态成员，不需要使用前缀，只需要在该成员所属的类中包含一个using static声明。  
```
using static System.Console;  //  在其他成员中包含Writeline()
using static System.Math  //  在其他成员中包含Sqrt()
...
WriteLine($"The square root of 16 is {Sqrt(16)}");
```
这等价于:
```
using System;
...
Console.WriteLine($"The square root of 16 is {Math.Sqrt(16)}");
```
### 静态成员的生存期
- 顺便一提，实例创建之后才能产生实例成员，在实例销毁之后实例成员也就不存在了。  
- 但是**即使类没有实例** ，也存在静态成员（在堆中）,并且可以访问。  

比如:
```
class D
{
  int Mem1;
  static public int Mem2;
}
...
static void Main()
{
  D.Mem =5;
  Console.WriteLine
    ("Mem2 = {0}", D.Mem2);
}
```
## 静态函数成员
- 如同静态字段。静态函数成员独立于任何类实例。即使没有类的实例，仍然可以调用静态方法。  
- 静态函数成员不能访问实例成员，但能访问其他静态成员。  
*如:静态方法的方法体访问静态字段* 
```
class X
{
  static public int A;  //  静态字段
  static public void PrintValA()  //  静态方法
  {
    Console.WriteLine("Value of A:{0}", A)
  }
}

在被调用的时候
class Program
{
  static void Main()
  {
    X.A = 10; //  使用点号语法
    X.PrintValA();  //  使用点号语法
  }
}
输出>> Value of A: 10
```
## 成员常量
与局部常量类似，不能再成员常量声明以后给它赋值
```
class MyClass
{
  const int IntVal; //  错误:必须初始化
  IntVal = 100; //  错误:不允许赋值
}
```

**与C和C++不同，在C#中没有全局常量。每个常量必须声明在类型内**  
另外，常量没有自己的存储位置，而是在编译时被编译器替换。这种方式类似于C和C++中的#define值。而且虽然常量成员表现得像静态值，但不能将常量声明
为static。  

## 属性
和字段不同，属性是一个**函数成员**   
- 它不一定为数据存储分配内存
- 它执行代码  

属性是一组两个匹配的、命名的、称为访问器的方法.  
- set访问器为属性赋值
- get访问器从属性获取值  

可以把set访问器想象成一个方法，带有单一的参数，它设置属性的值。get访问器没有参数，并从属性返回一个值。
* set访问器总是:
  * 拥有一个单独的、隐式的值参，名称为value,与属性的类型相同
  * 拥有一个返回类型void
* get访问器总是:
  * 没有参数；
  * 拥有一个与属性类型相同的返回类型。  
  
### 计算只读属性
get访问器计算出返回值，返回斜边长度
```
class RightTriangle
{
  public double A = 3;
  public double B = 4;
  public double Hypotenuse  //  只读属性
  {
    get{ return Math.Sqrt((A*A)+(B*B));}  //  计算返回值
  }
}

class Program
{
  static void Main()
  {
    RightTriangle c = new RightTriangle();
    Console.WriteLine($"Hyptenuse:{c.Hypotenuse}");
  }
}
```
## 实例构造函数
### 带参数的构造函数
* 构造函数可以带参数  
* 构造函数可以被重载
Class1有3个构造函数:一个不带参数，一个带Int参数，一个带string参数。
```
class Class1
{
  int Id;
  string Name;
  
  public Class1() {Id=28;Name="Nemo";}  //  构造函数0
  public Class1(int val)  {Id=val;Name="Nemo";} //  构造函数1
  public Class1(String name)  {Name=name;}  //  构造函数2
  
  public void SoundOff()
  {Console.WriteLine($"Name {Name},Id {Id}");}
}

class Program
{
  static void Main()
  {
    Class1 a = new Class1(),  //  调用构造函数0
           b = new Class1(7),  //  调用构造函数1
           c = new Class1("Bill");  //  调用构造函数2
    a.SoundOff();
    b.SoundOff();
    c.SoundOff();
  }
}
---------------------------------------
输出:
Name Nemo, Id 28
Name Nemo, Id 7
Name Bill, Id 0
---------------------------------------
```

## 静态构造函数
- 初始化类级别的项
  - 在引用任何静态成员之前
  - 在创建类的任何实例之前
- 静态构造函数在以下方面与实例构造函数类似
  - 静态构造函数的名称必须和类名相同
  - 构造函数不能返回值
- 静态构造函数在以下方面和实例构造函数不同
  - 静态构造函数声明中使用static关键字
  - 类只能有一个静态构造函数，而且不能带参数
  - 静态构造函数不能有访问修饰符

## 分部方法
分为**定义分部方法** 和**实现分分部方法** 
- 定义声明和实现声明的签名和返回类型必须匹配
  - 返回类型必须是void
  - 签名不能包括访问修饰符，这使分部方法是隐式私有的
  - 参数列表不能包含out参数
  - 在定义声明和实现声明中都必须包含上下文关键字partial,并且直接放在关键字void之前  
- 可以有定义部分而没有实现部分。这种情况，编译器把方法的声明以及方法内部任何对方法的调用都移除。不能只有分部方法的实现部分而没有定义部分
```
partial class MyClass
{
  partial void PrintSum(int x, int y);  //  定义分部方法
  public void Add(int x, int y)
  {
    PrintSum(x, y);
  }
}

partial class MyClass
{
  partial void PrintSum(int x, int y) //  实现分部方法
  {
    Console.WriteLine("Sum is {0}", x+y);
  }
}

class Program
{
  static void Main()
  {
    var mc = new MyClass();
    mc.Add(5,6)
  }
}

-------------------
输出结果: Sum is 11
-------------------
```
