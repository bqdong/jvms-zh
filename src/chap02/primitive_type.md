## 原子类型

Java 虚拟机支持的原子数据类型是数字类型（*numeric types*）、布尔类型(`boolean` type)（第 2.3.4 节）和 `returnAddress` 类型（第 2.3.3 节）。

数字类型由整数类型（*integral types*）（第 2.3.1 节）和浮点类型（*floating-point types*）组成（§2.3.2）。

整数类型包括：

- `byte`，它的值时8位有符号二进制补码整数，默认值为0。
- `short`，它的值时16位有符号二进制补码整数，默认值为0。
- `int`，它的值时32位有符号二进制补码整数，默认值为0。
- `long`，它的值时64位有符号二进制补码整数，默认值为0。
- `char`，它的值时16位**无符号**整数表示的在[基本多文种平面](https://zh.wikipedia.org/wiki/Unicode%E5%AD%97%E7%AC%A6%E5%B9%B3%E9%9D%A2%E6%98%A0%E5%B0%84#%E5%9F%BA%E6%9C%AC%E5%A4%9A%E6%96%87%E7%A7%8D%E5%B9%B3%E9%9D%A2)Unicode码点（code point），使用UTF-16编码，默认值是空码点（`\u0000`）。

浮点类型包括：

- `float`它的值与[IEEE 754 binary32](https://en.wikipedia.org/wiki/IEEE_754#Basic_and_interchange_formats)格式的相对应，默认值是**正数0**（+0）
- `double`，它的值与[IEEE 754 binary64](https://en.wikipedia.org/wiki/IEEE_754#Basic_and_interchange_formats)格式的相对应，默认值是**正数0**（+0）

`boolean`类型的值编码真值是`true`和`false`，默认值是`false`。

> 第一版的*The Java Virtual Machine Specification*没有将`boolean`类型视为一种Java虚拟机类型。然而，`boolean`值在Java虚拟机中有受限的支持。第二版的*The Java Virtual Machine Specification*将`boolean`视为一种类型来阐明这个问题。

`returnAddress` 类型的值是指向 Java虚拟机指令的操作码的指针。 在原子类型中，只有 `returnAddress` 类型不是直接与 Java 编程语言类型相关联。

### 整数类型和值

Java虚拟机的整数类型的值为：

- 对于`byte`，从-128~127（\\(-2^7\\)~\\(2^7-1\\)），包含127
- 对于`short`，从-32768~32767（\\(-2^15\\)~\\(2^15-1\\)），包含32767
