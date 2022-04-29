## 数据类型

与 Java 编程语言一样，Java 虚拟机在两种类型上进行操作：*原子类型（primitive types）*和*引用类型（reference types）*。相应地，有两种类型的值可以被存储在变量中，作为参数传递，作为方法返回值被返回，并可以对其进行操作：*原子类型的值（primitive values）*和*引用类型的值（reference values）*。

Java 虚拟机期望几乎所有类型检查都在运行前完成，通常这是由编译器完成的，而不必由 Java 完成虚拟机本身完成。原子类型的值不需要被标记（tagged）或以其他方式检查以确定它们在运行时的类型，或与引用类型的值进行区分。取而代之的是Java虚拟机的使用那些操作在特定类型上的指令来区分操作数类型。例如，*iadd*、*ladd*、*fadd* 和 *dadd* 都是 Java 虚拟机将两个数值相加并产生数值结果的指令，但每一个都是专门针对其操作数类型：分别为` int`、`long`、`float` 和 `double`。对于Java虚拟机指令集支持的类型的总结，请参阅 §2.11.1。

Java 虚拟机包含对对象的显式支持。一个对象是动态分配的类实例或数组。对对象的引用被认为时 Java 虚拟机引用（`reference`）类型。`reference`类型的值可以被认为是指向对象的指针。总是通过`reference`类型的值来操作、传递和测试对象。