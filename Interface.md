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
