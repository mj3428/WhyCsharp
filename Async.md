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
