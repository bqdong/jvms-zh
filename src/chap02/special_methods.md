# 特殊的方法

## 实例初始化方法

一个类有0个或过个实例初始化方法（instance initialization methods），通常每一个都对应一个使用Java语言写的构造方法。

一个方法是构造方法需要满足下面所有条件：

- 定义在类中（不是接口）
- 有特殊的名字 (`<init>`)
- 是 void 类型的

在一个类中，任何非void的名为 `<init>` 的方法不是实例初始化方法。在接口中，任何名为 `<init>` 的方法不是实例初始化方法。这些方法不能被Java虚拟机调用并且被格式检查所拒绝。

实例初始化方法的声明和使用被Java虚拟机限制。对于声明，方法的 `access_flags` 和 `code` 被约束。对于使用，实例初始化方法只被 `invokespecial` 指令调用。

> 因为 `<init>` 在Java语言中不是一个合法的标识符，它不能被使用Java语言程序直接使用。


## 类初始化方法（Class Initialization Methods）

一个类或接口最多拥有一个类或接口初始化方法，并由 Java 虚拟机（JVM）在初始化时调用该方法（§5.5）。

一个方法被视为类或接口初始化方法（`<clinit>`），当且仅当满足以下所有条件：

- 方法名必须为 `<clinit>`（特殊名称，非 Java 合法标识符）。

- 返回类型必须为 void（§4.3.3）。

- 在版本号 ≥ 51.0（Java SE 7+）的类文件中：必须设置 ACC_STATIC 标志（静态方法）。不能接受任何参数（Java SE 9+ 新增要求）。

> `ACC_STATIC`的要求在Java SE 7 中被引入，不接受参数在 Java SE 9中引入。类文件的版本号在 50.0 及以以下，名为 `<clinit>` 并且是void的方法被认为是类或接口初始化方法，不管是否设置 `ACC_STATIC` 或者接受参数。

类文件中其他名为 <clinit> 的方法不被视为初始化方法，且：JVM 不会调用这些方法。无法通过任何 JVM 指令调用（§4.9.1）。格式检查（verification）会拒绝此类方法（§4.6, §4.8）。

> 由于 `<clinit>` 不是合法的 Java 标识符，开发者无法直接在 Java 代码中声明该方法。它仅由编译器自动生成（如静态变量初始化或静态代码块 static { ... }）

## 签名多态方法（Signature Polymorphic Methods）

满足以下所有条件的方法称为签名多态方法（signature polymorphic）：

- 声明位置：必须在 `java.lang.invoke.MethodHandle` 类或 `java.lang.invoke.VarHandle` 类中声明。
- 参数类型：仅接受一个形式参数，类型为 `Object[]`。
- 修饰符标志：必须同时设置 `ACC_VARARGS`（可变参数）和 `ACC_NATIVE`（本地方法）标志。

Java 虚拟机（JVM）在 `invokevirtual` 指令（§invokevirtual）中对签名多态方法进行特殊处理，以实现以下功能：
动态调用方法句柄（method handle）；访问一个被`java.lang.invoke.VarHandle`实例所引用的变量。

方法句柄（method handle）是一种动态强类型、可直接执行的引用，指向底层方法、构造函数、字段或类似低级操作（§5.4.3.5），支持对参数或返回值进行可选转换（如类型适配、参数插入等）。
`java.lang.invoke.VarHandle`的实例是一种动态强类型引用，指向一个变量或变量族，包括：静态字段（static fields），非静态字段（non-static fields），数组元素（array elements），堆外数据结构组件（off-heap data structure components）。
更多细节请参阅 Java SE 平台 API 中的 `java.lang.invoke` 包。
