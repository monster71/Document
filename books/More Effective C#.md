
封闭泛型类型 ： 所有泛型类型都已确定的类型

开放泛型类型 ： 仅给出部分类型参数

### 在使用泛型时，泛型类型存不存在值类型有什么区别？ ###

> 使用多个引用类型参数的泛型类型并不会影响程序的内存占用，因为其被JIT编译后只生成一份代码。

> 不过若是包含至少一个值类型作为参数，那么其JIT编译后的代码则会各不相同。

### JIT编译泛型定义的过程(包含值类型作为类型参数) ###

1. 编译器将创建一个新的IL类，用来表示该封闭泛型类型

2. JIT将把该代码编译成x86指令

sp: JIT 并表示在某个类加载时就为其生成完整的x86指令，而是仅在类中的每个方法被第一次调用时才开始编译。

😎elicit: 

 运行时的需要额外内存占用：

1. 每种用值类型作为参数的封闭泛型类型保存一份IL定义的副本。

2. 每种用值类型作为参数的封闭类型保存一份所调用方法的机器码的副本。 

既然使用值类型会造成额外的内存开销，为什么会使用值类型作为泛型类型的参数？或者为什么不把值类型像引用类型一样只生成一份代码。

### 使用值类型作为泛型参数的好处 ###

> 避免了对于值类型的装箱和拆箱操作,这样也就降低了值类型的代码/数据所占用的空间。

✨ext：

	在java中泛型是编译安全的，但实际生成的并不存在所谓的泛型约束，而是通过编译器自动加上类型转换来实现泛型，相当于"伪泛型" 类似于一种语法糖。
	
	而在c#中泛型是一种特殊的类型，对于包不包含值类型作为类型参数的泛型，JIT有着不同的处理方式，而正是由于这些处理方式，实现了 避免装箱和拆箱。

	故c#与java的泛型是不同的。

> 此外，类型安全可以由编译器保住，也就让框架不必忙于进行运行时检查,进一步降低了代码量并提供了程序的性能。

**只有实际用到的方法才会被实例化。非泛型类中定义的泛型方法将不会被JIT编译**

> LINQ to Objects使用委托来执行，而LINQ to Sql 使用表达式树来实现。

### “长”弱引用和“短”弱引用 ###

> 短弱引用(short weak reference)将在其目标对象不存在时立即返回null。此时表示该对象要么已被回收，要么已被终结。

> 长弱引用(long weak reference)将在其目标对象仍在内存中依旧返回该对象。即使对象已被终结，其仍旧存在于内存中。这样，长弱引用对象有可能返回一个已经被终结过的对象指针。


----------

【summary】

 不够深入，但很全面，具有方向标的功能。