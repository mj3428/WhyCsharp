# 元数据概述
Program.exe包含名为Program的TypeDef.Program是公共密封类，从System.Object派生。Program还定义了两个方法:Main和.ctor(构造器)  
Main是公共静态方法，用IL代码实现（有的方法可能用本机CPU代码实现，比如x86代码）。Main返回类型是void，无参。构造器（名称始终是.ctor）是公共方法，也用IL代码实现。构造器
返回类型是void，无参，有一个this指针（指向调用方法是构造对象的内存）。  
