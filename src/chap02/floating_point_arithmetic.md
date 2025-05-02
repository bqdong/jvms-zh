# 浮点数运算

Java 虚拟机整合了 IEEE 754 浮点数标准的子集。

> 在Java SE 15 及之后，Java虚拟机使用2019版的 IEEE 754 标注。之前的Java虚拟机使用1985版的 IEEE 754 标准，32位格式被称作单精度浮点数，64位格式被称为双精度浮点数。


许多算术和类型转换的Java虚拟机指令支持浮点数。这些指令对应的 IEEE 754 操作如下表，除了下方特别描述的。

|指令|IEEE 754 操作|
|----|-------------|
| **`demp<op>`** , `femp<op>` |`compareQuietLess`（静默小于）,<br>`compareQuietLessEqual`（静默小于等于）,<br>`compareQuietGreater`（静默大于）,<br>`compareQuietGreaterEqual`（静默大于等于）,<br>`compareQuietEqual`（静默等于）,<br>`compareQuietNotEqual`（静默不等于） |
| **`dadd`** , **`fadd`**  | `addition`（加法） |
| **`dsub`** , **`fsub`**  | `subtraction`（减法） |
| **`dmul`** , **`fmul`**  | `multiplication`（乘法） |
| **`ddiv`** , **`fdiv`**  | `division`（除法） |
| **`dneg`** , **`fneg`**  | `negate`（取负） |
| **`i2d`** , **`i2f`** ,<br>**`l2d`** , **`l2f`**  | `convertFromInt`（从整数转换） |
| **`d2i`** , **`d2l`** ,<br>**`f2i`** , **`f2l`**  | `convertToIntegerTowardZero`（向零取整转换） |
| **`d2f`** , **`f2d`** | `convertFormat`（格式转换） |

Java虚拟机支持的浮点数运算和IEEE 754 标准的主要差异是：

- 浮点数余数指令 `drem` 和 `frem` 和 IEEE 754 的不对应。JVM 使用基于使用向0舍入策略的隐式除法；IEEE 754的余数指令则使用基于四舍五入的隐式除法。（舍入策略描述在后面）
- 浮点数取负指令`dneg` 和 `fneg` 和 IEEE 754 的取负操作并不是精确对应。尤其是指令不需要 NaN 的符号位翻转。
- Java虚拟机的浮点数指令不会抛出异常，陷入，或者其它符号行的 IEEE 754 不合法的异常操作，如除 0，溢出，下溢，或不精确。
- Java虚拟机不只是 IEEE 754 符号浮点数比较，并且没有符号性的NaN值。
- IEEE 754 包含舍入方向属性的舍入策略，Java虚拟机中没有对应的。Java虚拟机对于一个浮点数指令没有提供任何方法取改变舍入策略。
- Java虚拟机不支持 IEEE 754 定义的扩展的32位和扩展性的64位浮点数格式。超过指定的`float`类型和`double`类型的扩展的范围和精度在操作或者存储浮点数时不会使用。

> 一些 IEEE 754 操作没有对应的Java虚拟机指令，它们通过 `Math` 和 `StrictMath` 类提供，包含 IEEE 754 squareRoot 操作通过 `sqrt` 方法提供，IEEE 754 fusedMultiplyAdd 操作通过 `fma` 方法提供，IEEE 754 取余通过 `IEEEremainder` 方法提供。

Java虚拟机需要支持 IEEE 754 *subnormal* 浮点数和 *gradual underflow* ，这使证明特定数值算法的期望属性更加容易。

浮点运算是实数运算的近似模拟。实数有无限个，特定的浮点数格式只支持有限值。Java虚拟机中，舍入策略 (rounding policy) 是一个将实数映射成特定浮点数值的函数。对于在浮点数范围内可表示的实数，实数轴上的一个连续区间会被映射到单个浮点数值。在浮点格式中，数值上与某个浮点值相等的实数会被映射到该浮点值。例如，实数 1.5 在兼容的浮点格式下会被精确映射为浮点值 1.5。Java虚拟机定义了两种舍入策略，如下：

- 四舍五入（*round to nearest*）策略应用在所有浮点数指令上，除了：转换成整数值和余数。在四舍五入策略下，不精确的结果必须是舍入后无限精确结果最接近的值；如果有两个值的接近程度一样，那么最低位是0的那个将会被选中作为结果。四舍五入舍入策略对应 IEEE 754 的默认舍入方向属性，*roundTiesToEven*.
  > *roundTiesToEven* 舍入方向属性在1985版的 IEEE 754 标准中被称为四舍五入(round to nearest)舍入模式。Java虚拟机中的舍入策略是在这个舍入模式之后命名的。
- 向0舍入策略(*round toward zero*)应用在：1. 将浮点数转换成整数指令 *d2i*，*d2l*, *f2i*, *f2l* 中；2. 浮点数取余指令 *drem*，*frem* 。在向零取整（round toward zero）舍入策略下，不精确的结果会被舍入至最接近的、幅度不大于无限精度结果的可表示值。对于整数转换，向零取整策略等同于截断（直接丢弃小数部分的有效位）。向零取证的舍入策略对应 IEEE 754 中 *roundTowardZero* 舍入方向属性。
  > *roundTowardZero* 舍入方向属性在1985版的 IEEE 754 标准中被称为向零取整(round toward zero)舍入模式。Java虚拟机中的舍入策略是在这个舍入模式之后命名的。

Java 虚拟机（JVM）要求所有浮点运算指令必须将其结果舍入到目标精度。每条指令所使用的舍入策略为最近舍入（round to nearest）或向零舍入（round toward zero），具体采用哪种方式取决于上述规范。

> Java 1.0 和 Java 1.1 要求对浮点数表达式进行严格求值（Strict evaluation）。严格求值意思是每个 `float` 操作数对应一个 IEEE 754 32位格式可表示的值，每一个 `double` 操作数对应一个 IEEE 754 64位可表示的值，并且每一个浮点数操作符对应 IEEE 754的操作必须需匹配 IEEE 754中相同操作的结果。
>
> 严格求值（Strict evaluation）虽然能提供可预测的结果，但在 Java 1.0/1.1 时代的一些常见处理器架构上，会导致 Java 虚拟机（JVM）实现的性能问题。因此，从 Java 1.2 到 Java SE 16，Java SE 平台允许 JVM 实现为每种浮点类型关联 一个或两个值集（value sets）：
>
> float 类型 关联以下值集：
>
> - float 值集：对应 IEEE 754 二进制 32 位格式（binary32）可表示的值。
>
> - float-extended-exponent 值集：精度位数相同，但指数范围更大。
>
> double 类型 关联以下值集：
>
> - double 值集：对应 IEEE 754 二进制 64 位格式（binary64）可表示的值。
>
> - double-extended-exponent 值集：精度位数相同，但指数范围更大。
>
> 默认允许使用 扩展指数值集（extended-exponent value sets），缓解了某些处理器架构上的性能问题。
>
> 为了保持兼容性，Java 1.2 允许类文件（class file）通过 ACC_STRICT 标志 禁止 JVM 实现使用扩展指数值集。具体方式为：
> 在方法声明上设置 ACC_STRICT 标志，强制该方法内的浮点运算：float 操作数使用 float 值集（binary32）。double 操作数使用 double 值集（binary64）。
> 这样，标记为 ACC_STRICT 的方法的浮点语义与 Java 1.0/1.1 完全一致，确保计算结果的可预测性。
>
> 从 Java SE 17 开始，Java SE 平台始终要求严格求值（strict evaluation）浮点运算。过去在部分处理器架构上因严格求值导致的性能问题，在新一代硬件中已不复存在。因此，本规范不再将 float 和 double 类型与之前描述的四个值集（value sets）关联，且 ACC_STRICT 标志（对应 strictfp 关键字）不再影响浮点运算的求值方式。

