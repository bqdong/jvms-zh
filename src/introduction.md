# 第一章 介绍

## 历史

Java编程语言是一种通用的、并发的、面向对象的语言语言。它的语法类似于C和C++，但它省去了许多使C和C++变得复杂、混乱和不安全的特性。Java平台被开发，最初是用于解决构建网络设备的软件的问题。它被设计为支持多主机架构，并允许软件组件的安全交付。为了满足这些要求，编译后的代码必须满足要在跨网络传输中存活，在任何客户端上操作，并确保客户端是安全运行的目标。

万维网的普及使这些特性更加明显有趣。网络浏览器使数百万人能够以简单的方式上网并且访问丰富的媒体内容。终于有了一种媒介不管你使用的是哪种机器，无论连接的是快速的网络还是慢的调制解调器，看到和听到的内容基本上是相同的。

Web爱好者很快就发现了由Web的HTML所支持的内容文档格式太有限。HTML扩展，比如表单强调了这些限制，同时明确指出没有浏览器可以包含所有用户想要的功能。可扩展性是答案。

[HotJava](https://zh.wikipedia.org/wiki/HotJava)浏览器首先展示了Java编程语言和平台使程序嵌入在HTML页面成为可能的有趣的特性（译者注：[Java Applet](https://zh.wikipedia.org/wiki/Java_applet)）。程序和HTML页面被透明地下载到浏览器中，并且呈现在HTML页面中。在被浏览器接受之前，程序被仔细检查，以确保是安全的。像HTML页面，编译后的程序是独立于网络和主机的。这些程序不管来自哪里或使用哪种机器加载和运行，它们都表现出相同的行为。

与Java平台结合的Web浏览器不再局限于预先确定的功能集。访问者访问包含动态内容的网页可以确保其机器不会被该内容损坏。程序员只需要编写一次程序，就可以在任何提供Java运行时环境的机器上运行（译者注：Write once, run anywhere）。

## Java虚拟机

Java虚拟机是Java平台的基石。这是负责硬件和操作系统独立性的技术组件，其编译代码的体积小，且具有保护用户免受恶意程序的侵害的能力。

Java虚拟机是一个抽象的计算机。就像真正的计算一样机器，它有一个指令集，并在运行时操作各种内存区域。使用虚拟机来实现编程语言是相当常见的；最著名的虚拟机可能是[UCSD Pascal](https://zh.wikipedia.org/wiki/UCSD_Pascal) 的 [P-Code](https://zh.wikipedia.org/wiki/P-code%E6%9C%BA)虚拟机。

Java虚拟机的第一个原型实现，在 [Sun Microsystems](https://zh.wikipedia.org/wiki/%E6%98%87%E9%99%BD%E9%9B%BB%E8%85%A6)公司完成， 在软件中模拟了Java虚拟机指令集，该软件由一个类似于当代个人数码设备([PDA](https://zh.wikipedia.org/wiki/%E4%B8%AA%E4%BA%BA%E6%95%B0%E7%A0%81%E5%8A%A9%E7%90%86))的手持设备托管。Oracle当前的实现模拟了Java虚拟机在移动端，桌面端和服务器端的设备，但Java虚拟机不限定（not assume）任何特定的实现技术、主机硬件或主机操作系统。它不是固有的解释型的（ It is not inherently interpreted），但也可以通过将其指令集编译为处理器（silicon CPU）的指令集来实现。也可能可以在微指令（[microcode](https://zh.wikipedia.org/wiki/%E5%BE%AE%E7%A8%8B%E5%BA%8F)）或者硬件（silicon）中实现。

Java虚拟机对Java编程语言一无所知，仅针对特定的二进制格式，`class`文件格式。其中`class`文件包含Java虚拟机指令(或字节码(*bytecodes*))和符号表等辅助信息。

为了安全起见，对于`class`文件的代码，Java虚拟机引入了强大的语法和结构约束。然而，任何可以通过合法的`class`文件表示功能的语言都可以被托管到Java虚拟机（hosted by)。被一种普遍可用的、与机器无关的平台的特性所吸引的开发者，都可以转向Java虚拟机，将其作为他们语言的传递工具（Groovy，Kotlin）。

这里指定的Java虚拟机与Java SE 17平台兼容，并支持 [*The Java Language Specification, Java SE 17 Edition*](https://docs.oracle.com/javase/specs/jls/se17/html/index.html)中指定的Java编程语言。

## 内容组织

第2章概述了Java虚拟机架构。

第3章介绍了用Java编程语言编写的代码编译成Java虚拟机指令的过程。

第4章具体说明与硬件和操作系统无关用于表示已编译类和接口的二进制格式的`class`文件格式。

第5章具体说明Java虚拟机的启动，加载、链接和初始化类和接口。

第6章具体说明Java虚拟机的指令集，以操作码助记符的字母顺序来展示指令。

第7章给出了一个以操作码值为索引的Java虚拟机操作码助记符表。

在第二版的*Java Virtual Machine Specification*，第2章概述了 Java 编程语言，旨在支持
Java 虚拟机规范，但它本身并不是规范的一部分。在*Java Virtual Machine Specification, Java SE 17 Edition*（译者注：也就是本手册）中，请读者参阅[*The Java Language Specification, Java SE 17 Edition*](https://docs.oracle.com/javase/specs/jls/se17/html/index.html)来查询Java编程语言相关信息。引用的格式：(JLS§x.y)指出需要这样做的地方。

在第二版的Java虚拟机规范，第8章详细解释了具有共享主存的Java虚拟机线程交互的底层操作。在*Java Virtual Machine Specification, Java SE 17 Edition*中，读者可以参考[*The Java Language Specification, Java SE 17 Edition*](https://docs.oracle.com/javase/specs/jls/se17/html/index.html)的第17章有关线程和锁的信息。第17章反映了出自于JSR133专家组的[The Java Memory Model and Thread Specification](https://www.cs.umd.edu/~pugh/java/memoryModel/jsr133.pdf)。

## 符号

在整个规范中，我们指出的类和接口取自Java SE平台API。当我们引用一个类或接口时(除了那些声明在示例中的)，使用单个标识符*N*，其指的是`java.lang`包中的类或接口。在`java.lang`包以外的类或者接口，我们使用全限定名（fully qualified name）来指明。

每当我们引用一声明在`java`或它的任何子包中的类或者接口时，预期的引用是那个类或者接口被启动类加载器加载([§5.3.1](https://docs.oracle.com/javase/specs/jvms/se17/html/jvms-5.html#jvms-5.3.1))。

每当我们引用一个名为`java`的包的子包时，预期的引用是由启动类装加载器决定的。

本规范中字体的使用如下:

- `等宽`字体用于Java虚拟机数据类型、异常、错误、`class`文件结构、[Prolog](https://zh.wikipedia.org/wiki/Prolog)代码和Java代码片段。
- *斜体*是用于Java虚拟机的“汇编语言”，其操作码和操作数，以及Java虚拟机的运行时数据区域中的项。它也用于介绍新的术语和简单的强调。

非规范性信息，旨在澄清规范，使用以下格式表示：

> 这是非规范性信息。 它提供直觉、基本原理、建议、示例等。

## 反馈

读者可以报告*Java Virtual Machine Specification*中的技术错误和歧义，反馈至jls-jvms-spec-comments@openjdk.java.net（译者注：由于译者水平有限，此翻译可能存在不准确，反馈时请自行阅读原版规范确定无误后再反馈）。

关于`javac`（Java编程语言的参考编译器）生成和操作`class`文件的问题可以发送到compilerdev@openjdk.java.net（译者注：由于译者水平有限，此翻译可能存在不准确，反馈时请自行阅读原版规范确定无误后再反馈）。
