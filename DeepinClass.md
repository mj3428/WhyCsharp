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
