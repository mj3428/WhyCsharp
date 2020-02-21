# 枚举
每个枚举类型都有一个底层整数类型，默认为int
* 每个枚举成员都被赋予一个底层类型的常量值
* 在默认情况下，编译器对第一个成员赋值为0，对每一个后续成员赋的值都比以前出一个成员多1  
## 设置底层类型和显式值
可以把冒号和类型名放在枚举名之后，这样就可以使用int以外的整数类型，类型可以是任何整数类型。所有成员常量都属于枚举的底层类型。
```c#
enum TrafficLight : ulong
{...
```
## Flags特性
Flags特性不会改变计算结果，却提供了一些方便的特性。首先，它通知编译器、对象浏览器以及其他查看这段代码的工具，该枚举的成员不仅可以用
单独的值，还可以组合成位标志。这样浏览器就可以更恰当地解释该枚举类型的变量。  
其次，它允许枚举的ToString方法为位标志的值提供更多的格式化信息。ToString方法以一个枚举值位参数，将其与枚举的常量成员相比较。如果某个
成员相匹配，ToString返回该成员的字符串名称。  
举例：
```c#
enum CardDeckSettings : unit
{
  SingleDeck = 0x01,  //位0
  LargePictures = 0x02, //位1
  FancyNumbers = 0x04,  //位2
  Animation = 0x08  //位3
}

class Program{
  static void Main(){
    CardDeckSettings ops;
    ops = CardDeckSettings.FancyNumbers;  //  设置一个标志
    Console.WriteLine(ops.ToString());
    
    ops = CardDeckSettings.FancyNumbers | CardDeckSettings.Animation;
    Console.WriteLine(ops.ToString());  //  输出什么
  }
} 
---------------------------
FancyNumbers
12
---------------------------
```
这段代码中，Main做了以下事情:  
* 创建枚举类型CardDeckSettings的变量，设置一个位标志，并打印变量的值（即FancyNumbers）;  
* 为变量赋一个包含两个位标志的新值，并打印它的值(即12);  
作为第二次赋值结果而显示的值12是ops的值。它是一个int,因为FancyNumbers将位设置为4，Animation将位设置为值8，因此最终得到Int值12.在
赋值语句之后的WriteLine方法中，ToString方法会查找哪个枚举成员具有值12，幽羽没有找到，因此会打印出12。  
然而，如果在枚举声明前加上Flags特性，将告诉ToString方法位可以分开考虑。在查找值时，ToString会发现12对应两个分开的位标志成员——FancyNumbers
和Animation，这时将返回它们的名称，用逗号和空格隔开。  
结果如下:
```c#
----------------------------
FancyNumbers
FancyNumbers,Animation
----------------------------
```
