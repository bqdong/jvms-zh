## 运行时数据区域

Java虚拟机定义了在程序执行期间使用的各种运行时数据区域。其中一些数据区域是在Java虚拟机启动时创建的，只有在Java虚拟机退出时才会销毁。其他数据区域是属于单个线程的。单个线程的数据区域在线程创建时创建，在线程退出时销毁。

### `pc`寄存器

Java虚拟机可以一次支持许多执行线程（JLS§17）。每个 Java虚拟机线程都有自己的`PC`（程序计数器）寄存器。在任何时候，每个Java虚拟机线程都在执行单个方法的代码，即该线程的当前方法（§2.6）。如果该方法不是`native`方法，则`PC`寄存器包含当前正在执行的Java虚拟机指令的地址。如果当前由线程执行的方法是`native`方法，则Java 虚拟机的`PC`寄存器的值是未定义的。Java 虚拟机的`PC`寄存器足够宽，可以在特定平台上容纳`returnAddress`或者原生指针（native pointer）。

### Java 虚拟机栈

每个 Java 虚拟机线程都有一个私有的 Java 虚拟机堆栈（*Java Virtual Machine stack*），该虚拟机栈与线程同时创建。一个Java虚拟机栈存储着帧（frames）(§2.6)。Java虚拟机栈类似于传统语言如 C 语言的栈；它保存局部变量和部分结果，并在方法的调用和返回时发挥作用。因为除了对帧的 push 和 pop 操作外，Java 虚拟机栈永远不会被直接操作，所以帧可以在堆上分配。Java 虚拟机栈的内存不需要连续。

> 在第一版的 Java 虚拟机规范中，Java 虚拟机栈（Java Virtual Machine stack）被称为 Java 栈（*Java stack*）。

该规范允许 Java 虚拟机栈具有固定的大小，或者根据计算的需要动态地扩展和收缩。如果 Java 虚拟机栈的大小是固定的，则每个 Java 虚拟机栈的大小可以在创建该栈时独立选择。

> Java 虚拟机实现可以让程序员或用户控制 Java 虚拟机栈的初始大小，在动态扩展或收缩 Java 虚拟机栈的情况下，还可以控制最大和最小容量。

以下是与 Java 虚拟机栈相关的异常条件:

- 如果线程中的运算需要的 Java 虚拟机栈的空间超出允许的空间，Java 虚拟机将抛`StackOverflowError`。
- 如果 Java 虚拟机栈可以动态地扩展，且扩展时内存不足而影响到了扩展，或者内存不足以创建一个新线程的初始 Java 虚拟机栈，Java 虚拟机抛出一个`OutOfMemoryError`。

### 堆

Java 虚拟机有一个在所有 Java 虚拟机线程之间共享的堆（*heap*）。堆是分配所有类实例和数组的内存的运行时数据区域。

堆是在虚拟机启动时创建的。对象的堆存储空间由自动存储管理系统（称为垃圾收集器*garbage collector*）回收；对象从来不会被显式地回收。 Java 虚拟机不假定自动存储管理系统的特定类型，并且可以根据实现者的系统要求选择存储管理技术。堆可能具有固定尺寸，也可以按计算的要求扩展，如果不需要较大的堆，则可能会收缩。堆的内存不需要连续。

> Java 虚拟机实现可以提供给程序员或用户控制堆的初始大小，以及如果可以动态扩展或收缩堆，则可以控制最大和最小堆大小的功能。

以下异常条件与堆相关联：

- 如果计算需要的堆内存大小比自动存储管理系统所能提供的更多，则 Java 虚拟机会抛出`OutofMemoryError`。

### 方法区

Java 虚拟机有一个方法区（*method area*），该方法区在所有 Java 虚拟机线程中共享。该方法区类似于通常所说的编程语言的编译代码的存储区域或类似于操作系统进程中的[代码段](https://zh.wikipedia.org/wiki/%E4%BB%A3%E7%A0%81%E6%AE%B5)（“text" segment）。它存储了每个类结构，例如运行时常量池，成员变量（field）和方法数据以及方法和构造函数的代码，包括类和接口初始化以及实例初始化中使用的特殊方法（§2.9）。

方法区是在虚拟机启动时创建的。尽管方法区在逻辑上是堆（heap）的一部分，但简单的实现可能会选择不对该区域进行垃圾收集或压缩该区域（may choose not to either garbage collect or compact it）。该规范不要求方法区的位置或用于管理编译代码的策略。方法区可以具有固定大小，也可以根据计算的要求进行扩展，如果不需要较大的方法区域，则可能会收缩。方法区的内存不需要连续。

> Java 虚拟机的实现可以提供给程序员或用户对方法区初始大小的控制，以及在不同大小的方法区的情况下，控制最大和最小方法区大小的功能。

以下异常条件与方法区相关联：

- 如果方法去中的内存无法满足分配需求，则 Java 虚拟机将抛出`OutOfMemoryError`。

### 运行时常量池

运行时常量池（*run-time constant pool*）是每一个类或者接口的`class`文件中的`constant_pool`表的运行时表示（第4.4节）。它包含多种类型的常量，从编译时已知的数字常量到必须在运行时必须决定的方法和成员变量的引用。运行时常量池具有类似于常规编程语言的符号表的函数，但它包含的数据范围比典型的符号表更宽泛。

每个运行时常量池都是从 Java 虚拟机的方法区（§2.5.4）分配的。当 Java 虚拟机创建类或接口（§5.3）时，会创建类或接口的运行时常量池。

以下异常条件与类或接口的运行时常量池的创建有关：

- 创建类或接口时，如果运行时常量池的创建需要的内存大于 Java 虚拟机的方法区域的可用的内存，则 Java 虚拟机将抛出 `OutOfMemoryError`。

> 有关运行时常量池的创建的信息，请参见§5（加载，链接和初始化）。

### 本地方法栈

Java 虚拟机的实现可以使用常规的栈，称为“ C 栈”，以支持本地（native）方法（用Java编程语言以外的其他语言编写的方法）。本地方法栈也可以被用于 Java 虚拟机指令集解释器的实现的语言中，如 C 语言。不能加载`native`方法和不依赖于传统的栈的 Java 虚拟机实现没有必要提供本地方法栈。如果提供，当创建每个线程时，通常会分配本地方法栈。（An implementation of the Java Virtual Machine may use conventional stacks, colloquially called "C stacks," to support native methods (methods written in a language other than the Java programming language). Native method stacks may also be used by the implementation of an interpreter for the Java Virtual Machine's instruction set in a language such as C. Java Virtual Machine implementations that cannot load native methods and that do not themselves rely on conventional stacks need not supply native method stacks. If supplied, native method stacks are typically allocated per thread when each thread is created.）

该规范允许本地方法栈可以是固定大小，或者根据计算的要求动态扩展和收缩。如果本地方法栈的大小为固定大小，则在创建该栈时，可以独立选择每个本地方法栈的大小。

> Java 虚拟机实现可以提供给程序员或用户对本地方法栈初始大小的控制，以及在本地方法栈大小变动的情况下，可以控制最大和最小方法栈大小的功能。

以下异常条件与本地方法栈相关联：

- 如果线程中的计算需要的本地方法栈比允许的更大，Java 虚拟机将抛出`StackOverflowError`。
- 如果可以动态地扩展本地方法栈，并且尝试进行本地方法栈扩展，但是内存不足，或者没有足够的内存为新线程创建初始的本地方法堆栈，那么 Java 虚拟机将抛出`OutOfMemoryError`。