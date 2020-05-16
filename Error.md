# 异常
## try语句
- try块包含为避免出现异常而被保护的代码
- catch子句部分含有一个或多个catch子句。这些是畜栏里异常的代码段，它们也称为**异常处理程序**
- finally块含有在所有情况下都要执行的代码，无论有没有发生异常。  
## 异常过滤器
满足特定条件，这个条件被称为**过滤器**   
```c#
try
{
  ...执行某个web请求
}
catch(HttpRequestException e)when (e.Message.Contains("307"))
{
  ...采取某种行动
}
catch(HttpRequestException e)when (e.Message.Contains("301"))
{
  ...采取其他行动
}
```
when使用时需要包含一些重要特征:
- 它必须包含谓词表达式，该表达式的返回非真即假
- 他不能是异步的
- 不应使用任何需要长时间运行的操作
- 为此表达式中发生的任何异常都会被忽略。这使得调试谓词表达式变得更加困难，但它保留了调试原始应用程序错误所需的信息。  
