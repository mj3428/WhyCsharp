# 类型基础
实例字段，就是指非静态字段，有时也称为“实例成员”，简单地说，实例成员属于类的对象。
## 类型转换
### 使用C#的is和as操作符来转型
as操作符的工作方式与强制类型转换一样，只是它永远不抛出异常————相反，如果对象不能转型，结果就是null。  
`Employee e = o as Employee;  // 将o转换为Employee，会转型失败，不抛出异常，但e被设为null` 