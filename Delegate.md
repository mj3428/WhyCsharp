# 委托
## 声明委托类型
委托是类型，就好像类是类型一样。与类一样，委托类型必须在被用来创建变量以及类型的对象之前声明。`delegate void MyDel(int)`  
但是，委托类型声明在两个方面与方法声明不同:  
1. 以delegate关键字开头;  
2. 没有方法主体;  
> 虽然委托类型声明看上去和方法的声明一样，但它不需要再类内部声明，因为它是类型声明  

## 匿名方法
### 匿名方法的语法
* delegate类型关键字
* 参数列表，如果语句块没有使用任何参数则可以省略
* 语句块`delegate (Parameters){ImplementationCode}`  

## Lambda表达式
删除了delegate关键字改用lambda  
* 在参数列表和匿名方法主题之间放置lambda运算符`=>`。Lambda运算符读作"goes to".  
```c#
MyDel del = delgate(int x) {return x+1;}; //  匿名方法
MyDel del =        (int x) => {return x+1;};  //  lambda表达式
```
当然一下几种也是lambda表达式
```c#
MyDel del = delgate(int x) {return x+1;}; //  匿名方法
MyDel le1 =        (int x) => {return x+1;};  //  lambda表达式 有类型为显式类型
MyDel le2 =            (x) => {return x+1;};  //  lambda表达式 无类型为隐式类型
MyDel le3 =              x => {return x+1;};  //  lambda表达式
MyDel le4 =              x => x + 1;  //  lambda表达式
```  
参数类型当中，除非委托有ref或out参数————此时必须注明类型  
