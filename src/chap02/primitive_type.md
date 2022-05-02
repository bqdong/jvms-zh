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

- 对于`byte`，范围为-128~127（\\(-2^7\\)~\\(2^7-1\\)），包含-128和127
- 对于`short`，范围为-32768~32767（\\(-2^15\\)~\\(2^15-1\\)），包含-32768和32767
- 对于`int`，范围为-2147483648 ~2147483647（\\(-2^31\\)~\\(2^31-1\\)），包含-2147483648和2147483647
- 对于`long`，范围为-9223372036854775808~9223372036854775807（\\(-2^63\\)~\\(2^63-1\\)），包含-9223372036854775808和9223372036854775807
- 对于`char`，范围为0~65535，包含0和65535

### 浮点类型和值

浮点类型有`float`和`double`，它们在概念上分别和IEEE 754 中的32位binary32和64位binary64浮点数格式的值和操作相关联(JLS§1.7)。

> 在Java SE 15及之后的版本中，Java虚拟机使用IEEE 754的2019版标准。在Java SE 15之前，Java虚拟机使用的是1985版的IEEE  754标准，其中binary32格式被称为单精度（single format），binary64格式被称为双精度（double format）。

IEEE 754不仅包括由符号和绝对值（[*magnitude*](https://simple.wikipedia.org/wiki/Magnitude_(mathematics)#:~:text=The%20magnitude%20of%20a%20real%20number%20is%20usually,absolute%20value%20or%20modulus.%20It%20is%20written%20as)）组成的正数和负数，还有正零和负零，正无穷大和负无穷大(*infinities*)，和特殊的非数值的值(Not-a-Number)(以下简称NaN)。NaN值为用于表示某些无效运算的结果，如0除以0。`float`和`double`类型的NaN常量被预定义为`Float.NaN`和`Double.NaN`。

有限的非零的浮点数类型的值可以被表示为\\(s\cdot m \cdot 2^{(e-N+1)}\\) ，这里的：

- \\(s\\) 是 +1 或者 -1，
- \\(m\\) 是一个小于 \\(2^N\\) 的正整数，
- \\(e\\) 是一个整数，满足\\(E_{min}\le e \le E_{max}\\) ，其中 \\(E_{min}=-(2^{K-1}-2)\\) ，\\(E_{max}=2^{K-1}-1\\) ，
- \\(N\\) 和 \\(K\\) 是依赖于类型的参数

有些值可以以多种方式表示为这种形式。例如，假设一个浮点类型的值 \\(v\\)可以用确定的\\(s,m,e\\) 表示成这种形式，恰好其中\\(m\\)是偶数，\\(e\\)小于\\(2^{K-1}\\)， 则可以产生该值的另一种表示方式：\\(m\\)减半，\\(e\\)增加1。

如果\\(m \ge 2^{N-1}\\)，这种形式的表示称为*normalized*；否则，被称为*subnormal*。如果一个浮点类型的值不能够以\\(m \ge 2^{N-1}\\)这种形式表示，则称该值为非规格数([*subnormal number*](https://en.wikipedia.org/wiki/Subnormal_number))，因为它的绝对值（magnitude）低于最小的规格数（normalized value）。(译者注：具体的术语参考[wiki](https://en.wikipedia.org/wiki/IEEE_754)，这里翻译来自https://www.jianshu.com/p/43b1b09f27f4)

`float`和`double`中\\(N\\) 和 \\(K\\) 参数的约束（以及有它们衍生的 \\(E_{min}\\) 和 \\(E_{max}\\)）总结在<a href="#floating-point-parameters">表2.3.2-A</a>中：

<table width="60%" >
  <caption> <id id="floating-point-parameters">表2.3.2-A. 浮点数参数</id> </caption>
  <tr>
    <th>参数</th>
    <th><code>float</code></th>
    <th><code>double</code></th>
  </tr>
  <tr align="center">
    <td><math><mi>N</mi></math></td>
    <td>24</td>
    <td>53</td>
  </tr>
  <tr align="center">
    <td><math><mi>K</mi></math></td>
    <td>8</td>
    <td>11</td>
  </tr>
  <tr align="center">
    <td>
    <math>
      <msub>
        <mi>E</mi>
        <mrow>
          <mi>m</mi>
          <mi>a</mi>
          <mi>x</mi>
        </mrow>
      </msub>
    </math>
    </td>
    <td>+127</td>
    <td>+1023</td>
  </tr>
  <tr align="center">
    <td>
    <math>
      <msub>
        <mi>E</mi>
        <mrow>
          <mi>m</mi>
          <mi>i</mi>
          <mi>n</mi>
        </mrow>
      </msub>
    </math>
    </td>
    <td>-126</td>
    <td>-1022</td>
  </tr>
</table>


除了NaN，浮点值是有序的（*ordered*）。从小到大进行排列，分别是负无穷，负有限非零值，正零和负零，正有限非零值，正无穷。

IEEE 754允许它的binary32和binary64浮点格式有多个不同格式的NaN。然而，Java SE平台通常将一个给定的浮点类型的NaN值看作为一个单一的规范值（a single canonical value），因此，该规范通常引用任意NaN，就像引用一个规范值。

> 在IEEE 754下，用non-NaN参数进行浮点操作可能会产生NaN结果。IEEE 754指定了一组NaN位模式，但没有强制要求哪个特定的NaN位模式被用来表示NaN结果；这是留给了硬件体系结构。程序员可以创建具有不同位模式的NaN编码，例如，回溯性诊断信息（*A programmer can create NaNs with different bit patterns to
> encode, for example, retrospective diagnostic information.*）。这些NaN值可以是使用`Float.intBitsToFloat`和`Double.longBitsToDouble`创建，`float`对应`Float.intBitsToFloat`，`double`对应`Double.longBitsToDouble`。相反，要检查NaN值的位模式，`float`和`double`类型可以分别使用`Float.floatToRawIntBits`和`Double.doubleToRawLongBits`方法。

正0和负0比较起来是相等的，但是还有其他的运算可以区分它们；例如，用1.0除以0.0得到正无穷，但是用1.0除以-0.0得到负无穷。

NaN是无序的（*unordered*），因此，如果操作数有一个或两个都是NaN，数值比较和数值相等的测试都为`false`。特别的，数值相等性测试中值与自身的数值相等的结果为`false`，当且仅当该值为NaN。数值的不等性测试中，如果任何一个操作数是NaN，测试结果为`true`。

### `returnAddress`类型和值

`returnAddress`类型被用于Java虚拟机的*jsr*、*ret*和*jsr_w*指令(*§jsr、§ret、§jsr_w*)。`returnAddress`类型的值是指向Java虚拟机指令操作码的指针。不像数字型的原子类型，`returnAddress`类型没有和Java编程语言的任何类型相对应，而且不能够被正在运行的程序修改。

### `boolean`类型

虽然 Java 虚拟机定义了`boolean`类型，但提供给其的支持非常有限。 没有单独的 Java 虚拟机指令专用于对布尔值的操作。 相反，Java 编程语言中操作在`boolean`值上的表达式会被编译成使用Java虚拟机的`int`数据类型。

Java 虚拟机确实直接支持`boolean`数组。 它的`newarray`指令（*§newarray*）允许创建`boolean`数组。 `boolean`类型的数组可以使用`byte`数组的*baload*和*bastore*指令来实现访问和修改（*§baload, §bastore*）。

> 在 Oracle 的 Java 虚拟机实现中，Java编程语言的`boolean`数组被编码成Java虚拟机的`byte`数组，每个`boolean`元素使用8位（8 bits）来表示。

Java虚拟机对`boolean`数组进行编码时，使用1表示`true`，使用0表示`false`。Java编程语言`boolean`值被编译器映射到Java虚拟机的`int`类型的地方，编译器必须使用相同的编码。

