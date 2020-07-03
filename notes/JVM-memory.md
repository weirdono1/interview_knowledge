

# 运行时数据区域



![image-20200703170643997](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200703170643997.png)





## 程序计数器(Program Counter Register)

程序计数器是一块较小的内存空间, 是当前线程所执行的字节码的行号指示器，每条线程都要有一个独立的程序计数器，这类内存也称为“线程私有”的内存。

正在执行java方法的话，计数器记录的是虚拟机字节码指令的地址（当前指令的地址）。如果还是Native方法，则为空。

这个内存区域是唯一一个在虚拟机中没有规定任何OutOfMemoryError情况的区域

## Java虚拟机栈（Java Virtual Machine Stacks）

虚拟机栈是描述java方法执行的内存模型，每个方法在执行的同时都会创建一个栈帧（Stack Frame）用于存储局部变量表、操作数栈、动态链接、方法出口等信息。每一个方法从调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中入栈到出栈的过程。

栈帧（ Frame）是用来存储数据和部分过程结果的数据结构，同时也被用来处理动态链接 (Dynamic Linking)、 方法返回值和异常分派（ Dispatch Exception）。栈帧随着方法调用而创建，随着方法结束而销毁——无论方法是正常完成还是异常完成（抛出了在方法内未被捕获的异常）都算作方法结束。

## 本地方法栈（Native Method Stack）

本地方法区和Java Stack（虚拟机栈）作用类似, 区别是虚拟机栈为执行Java方法服务, 而本地方法栈则为Native方法服务, 如果一个VM实现使用C-linkage模型来支持Native调用, 那么该栈将会是一个C栈，但HotSpot VM直接就把本地方法栈和虚拟机栈合二为一。

## 堆（Heap）

是被所有线程共享的一块内存区域，创建的对象和数组都保存在Java堆内存中，也是垃圾收集器进行垃圾收集的最重要的内存区域。由于现代VM采用分代收集算法, 因此Java堆从GC的角度还可以细分为: 新生代(Eden区、From Survivor区和To Survivor区)和老年代。

## 方法区（Method Area）

即我们常说的永久代(Permanent Generation), 用于存储被JVM加载的类信息、常量、静态变量、即时编译器编译后的代码等数据. HotSpot VM把GC分代收集扩展至方法区, 即使用Java堆的永久代来实现方法区, 这样HotSpot的垃圾收集器就可以像管理Java堆一样管理这部分内存, 而不必为方法区开发专门的内存管理器(永久带的内存回收的主要目标是针对常量池的回收和类型的卸载, 因此收益一般很小)。

## 运行时常量池（Runtime Constant Pool）

运行时常量池（Runtime Constant Pool）是方法区的一部分。Class文件中除了有类的版本、字段、方法、接口等描述等信息外，还有一项信息是常量池。用于存放编译期生成的各种字面量和符号引用，这部分内容将在类加载后进入方法区的运行时常量池中存放。

## 直接内存 （Direct Memory）

在 JDK 1.4 中新引入了 NIO 类，它可以使用 Native 函数库直接分配堆外内存，然后通过 Java 堆里的 DirectByteBuffer 对象作为这块内存的引用进行操作。这样能在一些场景中显著提高性能，因为避免了在堆内存和堆外内存来回拷贝数据。