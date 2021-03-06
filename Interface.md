# 接口
## 什么是接口
直接上实例
```c#
interface IInfo
{
  string GetName();
  string GetAge();
}
class CA:IInfo
{
  public string Name;
  public string Age;
  public string GetName() {return Name;}
  public string GetAge() {return Age.ToString();}
}
class CB:IInfo
{
  public string First;
  public string Last;
  public double PersonsAge;
  public string GetName() {return First + " " + Last;}
  public string GetAge() {return PersonsAge.ToString();}
}
class Program
{
  static void PrintInfo(IInfo item)
  {
    Console.WriteLine("Name:{0}, Age{1}",item.GetName(), item.GetAge());
  }
  
  static void Main()
  {
    CA a = new CA() {Name = "John Doe",Age = 35};
    CB b = new CB() {First = "Jane", Last = "Doe", PersonsAge = 23};
    
    PrintInfo(a);
    PrintInfo(b);
  }
}
```

## 声明接口
* 声明接口不能包含以下成员:  
  * 数据成员
  * 静态成员
* 接口声明只能包含如下类型的**非静态** 成员函数的声明
  * 方法、属性、事件、索引器
* 这些函数成员的声明不能包含任何实现代码，必须使用分号代替每一个成员声明的主体。  
* 按照惯例，接口名称必须从大写的I开始（比如ISaveable）
* 与类和结构一样，接口声明也可以分隔成分部接口声明  
比如:
```c#
interface IMyinterface1
{
  int Dostuff (int nVar1, long lVar2);  //  分号代替了主体
  double DoOtherStuff(string s, long x);  //  分号代替了主体
}
```

另外，**接口的访问性** 和 **接口成员的访问性** 有一些重要区别:  
* 接口声明可以有任何的访问修饰符:public\protected\internal或private
* 然而，接口成员是隐式public的，不允许有任何访问修饰符，包括public
```c#
public interface IMyInterface2 // 接口可以有访问修饰符
{
  private int Method1(int nVar1, long lVar2); //  错误，接口成员不允许有访问修饰符
}
```
## 实现接口
类之后冒号接口，然后..用就行  
关于实现接口，需要了解重要事项如下:
* 如果类实现了接口，它必须实现接口的**所有成员**   
* 如果类派生自基类并实现了接口，基类列表中的基类名称必须放在所有接口之前，只能有一个基类
```c#
class Derived : MyBaseClass(第一个为基类), IIfc1, IEnumerable, IComparable（后面3个为接口）
{
  ...
}
```
### 简单接口示例
```c#
interface IIfc1 //  声明接口
{
  void PrintOut(string s);
}

class MyClass : IIfc1 //  声明类
{
  public void PrintOut(string s)  //  实现
  {
    Console.WriteLine($"Calling through:{s}");
  }
}

class Program
{
  static void Main()
  {
    MyClass mc = new MyClass(); //  创建实例
    mc.PrintOut("object");  //  调用方法
  }
}
```
## 接口是引用类型
虽然不能直接访问类对象的成员。然而，我们可以通过把类对象引用强制转换为接口类型来获取**指向接口的引用** 。一旦有了接口的引用，就可以
使用点语法来调用接口的成员。  
```c#
IIfc1 ifc = (IIfc1) mc; //  获取接口的引用
ifc.PrintOut("interface");  //  使用接口的引用调用方法
```
* 第一个语句中，mc变量是一个实现了IIFc1接口的类对象的引用。该语句讲该引用显式转换为指向接口的引用，并将它赋值给ifc。但是，我们可以省略
显式转换的部分，因为编译器可以隐式地把它转换成正确的接口，而正确的接口可以从赋值语句的左端推断出来  
* 在第二个语句中，使用指向接口的引用来调用实现方法。  

接口类型的引用可以直接把接口调出来，并使用其成员
```c#
interface IIfc1 //  声明接口
{
  void PrintOut(string s);
}

class MyClass : IIfc1 //  声明类
{
  public void PrintOut(string s)  //  实现
  {
    Console.WriteLine($"Calling through:{s}");
  }
}

class Program
{
  static void Main()
  {
    MyClass mc = new MyClass(); //  创建类对象
    mc.PrintOut("object");  //  调用类对象的实现方法
    
    IIfc1 ifc = (IIfc1)mc;  //  将类对象的引用转换为接口类型的引用
    ifc.PrintOut("interface");  //  调用接口方法
  }
}
```
## 好用的as运算符
如果尝试将类对象引用强制转换为类未实现的接口的引用，强制转换操作会抛出一个异常。可以通过使用as运算符来避免这个问题。  
* 如果类实现了接口，表达式返回指向接口的引用  
* 如果类美欧实现接口，表达式返回null而不是抛出异常  
第一行使用as运算符来从类对象获取接口引用。表达式的结果会把b的值设置为null或ILiveBirth接口的引用  
第二行代码检测了b的值，如果它不是null，则执行命令来调用接口成员方法。  
```c#
ILiveBirth b = a as ILiveBirth; //  跟cast:(ILiveBirth)a 一样
if (b != null)
Console.WriteLine($"Baby is called:{b.BabyCalled()}")
```
## 实现具有重复成员的接口
要是两个接口内容一样，就实现一个
```c#
class MyClass : IIfc1,IIfc2 //  实现两个接口
{
  public void PrintOut(string s)  //  两个接口的单一实现
  {
    Console.WriteLine($"Calling through:{s}");
  }
}

class Program
{
  static void Main()
  {
    MyClass mc = new MyClass();
    mc.PrintOut("object");
  }
}
```
## 帅气的实现接口（派生成员作为实现）
1. IIfc1是一个具有PrintOut方法成员的接口
2. MyBaseClass包含了一个叫做PrintOut的方法，它和IIfc1的方法声明相匹配  
3. Derived类有一个空的声明主体，但它派生自MyBaseClass，并在基类列表中包含了IIfc1  
4. 即使Derived的声明主体是空的，基类中的代码还是能满足实现接口方法的需求。  
```cpp
interface IIfc1 {void PrintOut(string s);}

class MyBaseClass //  声明基类
{
  public void PrintOut(string s)  //  声明方法
  {
    Console.WriteLine($"Calling through:{s}");
  }
}
class Derived : MyBaseClass,IIfc1 // 声明类
{
}

class Program{
  static void Main()
  {
    Derived d = new Derived();  //  创建类对象
    d.PrintOut("object.");  //  调用方法
  }
}
```
## 显式接口成员实现
* 与所有接口实现相似，位于实现了接口的类或结构中  
* 它使用限定接口名称来声明，由接口名称和城影院名称以及它们中间的点分隔符号构成  
```c#
class MyClass : IIfc1,IIfc2
{
  void IIfc1.PrintOut(string s) //  显式实现
  {...}
  
  void IIfc2.PrintOut(string s) //  显式实现
  {...}
}
```
