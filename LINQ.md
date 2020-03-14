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
## LINQ TO XML
### XML类
- XDocument节点可以有以下直接子节点
  - 下面的每一个节点类型，最多有一个：XDeclaration节点、XDocumentType节点以及XElement节点
  - 任何数量的XProcessingInstruction节点
- 如果在XDocument下有最高级别的XElement节点，那么它就是XML树中其他元素的根
- 根元素可以包含任意数量的嵌套XElement、XCpmment或XProcessingInstruction节点，并且可以在任何级别上嵌套。  
#### 增加节点以及操作XML
```c#
using System;
using System.Xml.Lingq;

class Program
{
  static void Main()
  {
    XDocument xd = new XDocument( //  创建XML树
      new XElement("root",
        new XElement("first")
      )
    );
    
    Console.WriteLine("Original tree");
    Console.WriteLine(xd);Console.WriteLine();  //  显示树
    XElement rt = xd.Element("root"); //  获取第一个元素
    
    rt.Add(new XElement("second"));
    rt.Add(new XElement("thrid"), //  再添加3个元素
           new XComment("Important Comment"),
           new XElement("fourth"));
           
    Console.WriteLine("Modifed tree");
    Console.WriteLine(xd);  //  显示Modified tree
  }
}
```
Add方法把新的子节点放在既有子节点之后，但把节点放在子节点之前或者之间也是可以的，使用AddFirst、AddBeforeSelf和AddAfterSelf方法即可。  
### LINQ to XML的LINQ查询
示例:  
- 它从XML树种选择那些名字有5个字符的元素。幽羽这些元素的名字是first、second和thrid，只有first和third这两个名字复合搜索标准，因此这些节点被选中。
- 它显示了所选元素的名字
- 它格式化并显示了所选节点，包括节点名以及特性值。注意特性使用Attribute方法来获取，特性的值使用Value属性来获取。  
```c#
static void Main()
{
  XDocument xd = XDocument.Load("SimpleSample.xml") //加载文档
  XElement rt = xd.Element("MyElements"); //获取根元素
  
  var xyz = from e in rt.Elements() //  选择名称包含
            where e.Name.ToString().Length == 5 //  5个字符的元素
            select e;
  foreach (XElement x in xyz)
    Console.WriteLine(x.Name.Tostring()); //  显示所选的元素
  
  Console.WriteLine();
  foreach (XElement x in xyz)
    Console.WriteLine("Name:{0}, color:{1}, size:{2}",
                      x.Name,
                      x.Attribute("color").Value,
                      x.Attribute("size").Value);
}
```
