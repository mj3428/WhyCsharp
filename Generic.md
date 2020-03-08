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
