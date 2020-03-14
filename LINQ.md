# LINQ
## 什么LINQ
从对象获取数据的方法一直是作为程序的一部分专门设计的。然而使用LINQ可以轻松地查询对象集合。  
LINQ的高级特性：  
- LINQ发音link，代表语言集成查询(language integrated query)
- LINQ是.NET框架的扩展，它允许我们以使用SQL查询数据库的方式来查询数据集合  
- 使用LINQ，你可以从数据库，对象集合以及XML文档中查询数据  
```c#
static void Main()
{
  int[] numbers = {2, 12, 5, 15}
  
  IEnumerable<int> lowNums =  //  定义并存储查询
    from n in numbers
    where n < 10
    select n;
  
  foreach (var x in lowNums)  //  执行查询
    Console.Write($"{x},");
}
--------------------------------------------
结果:
2, 5
--------------------------------------------
```
## select...group子句
- select子句指定应该选择所选对象的哪些部分。它可以指定下面的任意一项。  
  - 整个数据项
  - 数据项的一个字段
  - 数据项中几个字段组成的新对象  
- group..by子句是可选的，用来指定选择的项如何被分组。
```c#
using System;
using System.Linq;
class Program{
  static void Main(){
    var students = new[]  //  匿名类型的对象数组
    {
      new {LName = "Jones", FName = "Mary", Age = 19, Major="History"},
      new {LName = "Smith", FName = "Bob", Age = 20, Major="CompSci"},
      new {LName = "Fleming", FName = "Carol", Age = 21, Major="History"}
    };
    
    var query = from s in students
                select s;
    foreach (var q in query)
      Console.WriteLine(q);
}
```
## 查询中的匿名类型
用select new关键字来创建匿名类型
```c#
using System;
using System.Linq;

class Program
{
  static void Main()
  {
    var students = new[]  //  匿名类型对象数组
    {
      new {LName = "Jones", FName = "Mary", Age = 19, Major="History"},
      new {LName = "Smith", FName = "Bob", Age = 20, Major="CompSci"},
      new {LName = "Fleming", FName = "Carol", Age = 21, Major="History"}
    };
    
    var query = from s in students
                select new {s.LName,s.FName,s.Major};
                
    foreach (var q in query)
      Console.WriteLine($"{q.FName}{q.LName}--{q.Major}");
  }
}
```
