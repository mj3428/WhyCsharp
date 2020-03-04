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

