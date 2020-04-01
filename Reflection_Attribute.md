# 反射和特性
## Type类
Type是抽象类，因此它不能有实例。在运行时，CLR创建从Type派生的类的实例，Type包含了类型信息。当访问这些时，CLR不会返回派生类的引用而是返回
Type基类的引用。  
Type的重要事项:  
- 对于程序中用到的每一个类型，CLR都会创建一个包含这个给类型信息的Type类型的对象。  
- 不管创建的类型有多少个实例，只有一个Type对象会关联到所有这些实例。  
## 获取Type对象
使用实例对象的GetType方法和typeof运算符和类名来获取Type对象。object类型包含了一个叫作GetType的方法，它返回对实例的Type对象的引用。由于每一
个类型最终都是从object派生的，所以我没可以在任何类型的对象上使用GetType方法来获取它的Type对象，如:`Type t = myInstance.GetType();`
