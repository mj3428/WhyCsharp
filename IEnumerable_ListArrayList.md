# IEnumerable、IEnumerator、List、ArrayList
ICollection主要针对静态集合；IList主要针对动态集合  
IEnumerable<T>继承自IEnumerable  
ICollection<T>继承自IEnumerable<T>  
IList<T>继承自ICollection<T>  
IEnumerable接口  
实现了IEnumerable接口的集合表明该集合能够提供一个enumerator(枚举器)对象，支持当前的遍历集合。IEnumerable接口只有一个成员GetEnumerator()方法。  
IList接口和ArrayList类的目的是实现动态数组，ArrayList是IList的一个实现。  
List<T>是ArrayList的泛型，ArrayList里边的数据类型是object，List<T>里边的是具体的某种类型，ArrayList类似于向量，可以存储不同的数据类型在一个数组里边（转换为了object）。  

一般使用的时候尽量使用List<T>，因为ArrayList存取都要进行一次转换。  

[]类型的数组类似于List<T>，不同的是[]是定长的，而List<T>是长度可变的数组  

IEnumerable表明对象是不确定类型的集合并支持简单迭代，是不是定长根本不关心...  

IEnumerable<T>表明对象是指定类型的集合并支持简单迭代，是不是定长根本不关心...  

ICollection是IEnumerable接口的派生接口，表明对象是不确定类型的集合并支持简单迭代，而且定义了集合的大小、枚举数和同步方法，这里的大小是指可以是定长的也可以是不定长的...  

IList是ICollection和IEnumerable的派生接口，表明对象是不确定类型的集合并支持简单迭代，而且定义了集合的大小、枚举数和同步方法，还可以按照索引单独访问，
这里的大小是指可以是定长的也可以是不定长的...  

ArrayList类是IList接口的实现，表明对象是不确定类型的大小可按需动态增加的数组...  

List<T>类是IList<T>接口的实现，是ArrayList类的泛型等效类并增强了功能，表明对象是可通过索引访问的对象的强类型列表...在.NET 2.0以上可以完全代替ArrayList，就是说ArrayList已经被淘汰...  

而动态数组和链表在本质上是不同的...在.NET 2.0以上有双向链表LinkedList<T>泛型类，它也是继承自ICollection<T>,IEnumerable<T>,ICollection,IEnumerable...  
