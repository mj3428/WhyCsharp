# 泛型
## Csharp中的泛型
泛型generic特性提供了一种更优雅的方式，可以让多个类型共享一组代码。泛型允许我们**声明类型参数化** 的代码，用不同的类型进行实例化。  
举例创建一个示例：
```c#
class MyStack<T>
{
  int StackPointer = 0;
  T [] StackAarray;
  
  public void Push(T x){...}
  
  public void Pop(){...}
  ...
}
```
1. 在MyStack类定义中，使用类型占位符T而不是float来替换int
2. 修改类名称为MyStack  
3. 在类名后放置<T>  
### 声明泛型类
* 在类名之后放置一组尖括号  
* 在尖括号中用逗号分隔的占位符字符串来表示需要提供的类型，这叫做 **类型参数**  
* 在泛型类声明的主体中使用类型参数来表示替代类型  
使用时:
```c#
class SomeClass <T1, T2>
{
  public T1 SomeVar;
  pubilc T2 OtherVar;
}
```
## 类型参数的约束
需要提供额外的信息让编译器知道参数可以接受哪些类型。  
### where语句
where语句要点：
- 它们在类型参数列表的关闭尖括号之后列出
- 它们不适用逗号或其他符号分隔  
- 他们可以以任何次序列出
- where是上下文关键字，所以可以在其他上下文中使用  
```c#
class MyClass <T1, T2, T3>
      where T2:Custmoer //  T2的约束 没有分隔符
      where T3:IComparable  //  T3的约束 没有分隔符
{
  ...
}
```

### 调用泛型方法
```c#
void Dostuff<T1, T2>(T1 t1,T2 t2)
{
  T1 someVar = t1;
  T2 otherVar = t2;
  ...
}
DoStuff<short, int>(sVal, iVal);
DoStuff<int, long>(iVal, lVal);
```
## 泛型结构
直接上例子
```c#
struct PieceOfData<T> //  泛型结构
{
  public PieceOfData(T value){_data = value;}
  private T _data;
  public T Data
  {
    get {return _data;}
    set {_data = value;}
  }
}

class Program
{
  static void Main()
  {
    var int Data = new PieceOfData<int>(10);
    var stringData = new PieceOfData<string>("Hi there.")
    
    Console.WriteLine($"intData = {intData.Data}");
    Console.WriteLine($"stringData = {stringData.Data}");
  }
}

-------------------------------------
intData = 10;
stringData = Hi there
-------------------------------------
```
