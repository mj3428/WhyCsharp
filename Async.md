# 异步
## 什么是异步
重要点:  
- 默认情况下:一个进程只包含一个线程，从程序的开始一直执行到借宿
- 线程可以派生其他线程，因此在任意时刻，一个进程都可能包含不同状态的多个线程，它们执行程序的不同部分  
- 如果一个进程有多个线程，他们将共享进程的资源
- 系统为处理器执行所调度的单元是线程，不是进程。  
## async/awaitt特性的结构
- 调用方法:该方法调用异步方法，然后在异步方法执行其任务的时候继续执行
- 异步方法:该方法异步执行其工作，然后立即返回到调用方法  
- await表达式:用于异步方法内部，指明需要异步执行的任务。  
```c#
class Program
{
  static void Main()
  {
    ...
    Task<int> value = DoAsynStuff.CalculateSumAsync(5,6);
    ...
  }
  
  static class DoAsyncStuff
  {
    public static async Task<int> CalculateSumAsync(int i1, int i2)
    {
      int sum = await TaskEx.Run(() => GetSum(i1, i2));
      return sum;
    }
  }
}
```
### 异步方法的控制流
- 第一个await表达式之前的部分:从方法开头到第一个await表达式之前的所有代码。  
- await表达式:表示将被异步执行的任务
- 后续部分:await表达式之后的方法中的其余代码。  
```c#
async Task<int> CountCharactersAsync(int id, string site)
{
  Console.WriteLine("Starting CountCharaters");
  WebClient wc = new WebClient();
  string result = await wc.DownloadStringTaskAsync(new Uri(site));
  Console.WriteLine("CountCharacters Completed");
  return result.Length;
}
```
## BeginInvoke和EndInvoke
- 在调用BeginInvoke时，参数列表中的实际参数组成如下：
  - 引用方法需要的参数;
  - 两个额外的参数__callback参数和state参数;
- BeginInvoke从线程池中获取一个线程并且让引用方法在心动线程中开始运行
- BeginInvoke返回给调用线程一个实现IAsyncResult接口的对象的引用。这个接口引用包含了在线程池程中运行的异步方法的当前状态。然后原始线程可以继续
  执行。  

```c#
delegate long MyDel(int first, int second); //  委托声明
...
static long Sum(int x, int y){return x+y;}  //  方法匹配委托
...
MyDel del = new MyDel(Sum); //  创建委托对象
IAsyncResult iar = del.BeginInvoke(3, 5, null, null);
```
EndInvoke方法使用异步方法调用返回的值，并且释放线程使用的资源。EndInvoke有如下特性:
- 它接受一个由BeginInvoke方法返回的IAsyncResult对象的引用作为参数，并找到它关联的线程
- 如果线程池的线程已经退出，则EndInvoke做如下的事情
  - 清理退出线程的状态并释放其资源
  - 找到引用方法返回的值并把它作为返回值返回  
- 如果当EndInvoke被调用时线程池的线程仍然在运行，调用线程就会停止并等待它完成，然乎再清理并返回值。因为EndInvoke是为开启的线程进行清理，
  所以必须确保对每一个BeginInvoke都调用EndInvoke
- 如果异步方法触发了异常，则在调用EndInvoke时抛出异常  
直接上案例：
```c#
using System;
using System.Threading; //  For Threading.Sleep()
delegate long MyDel(int first, int Second); //  声明委托类型
class Program{
  static long Sum(int x, int y) //  声明异步方法
  {
    Console.WriteLine("       Inside Sum");
    Thread.Sleep(100);
    return x+y;
  }
  static void Main(){
  MyDel del = new MyDel(Sum);
  Console.WriteLine("Before BeginInvoke");
  IAsyncResult iar = del.BeginInvoke(3,5,null,null);  //  开始异步调用
  Console.WriteLine("After BeginInvoke");
  Console.WriteLine("Doing stuff");
  long result = del.EndInvoke(iar); //  等待借宿并获取结果
  Console.WriteLine($"After EndInvoke:{result}");
  }
}
```
