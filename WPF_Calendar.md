# 日历控件
在私人函数的接口，接收到参数sender后，sender包含非常多的参数，要使用其中一个信号的时候，要使用sender的参数，就必须要将sender进行
类型转换后可调用参数。  
```c#
Calendar calendar = sender as Calendar
if(calendar == null) return;
```
