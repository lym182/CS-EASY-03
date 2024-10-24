# 2024090914025-李伊眉-CS-03
## 问题回答
### 一.C语言内存模型
1. 栈溢出是一种程序运行时错误。当程序在栈上分配的空间超过了栈的最大容量时，就会发生栈溢出。

2. - **栈区**由编译器自动管理；
     **堆区**需要程序员手动用函数申请内存，使用完后也要手动释放，否则会内存泄漏。
   - **栈区**从高地址向低地址增长，遵循后进先出原则；
     **堆区**从低地址向高地址增长，分配和释放不连续，易产生内存碎片。
   - **栈区**大小有限，易溢出；
     **堆区**理论上受物理和虚拟内存限制，空间较大，但可能出现内存碎片。
   - **栈区**存储函数临时数据，生命周期与函数执行周期相关；
     **堆区**存储动态分配的数据，生命周期由程序员控制，适合长期使用或编译时不确定的数据。

3. - **只读**：常量区，代码区。
   - **可读写**：栈区，堆区，全局区（静态区）。

4. - malloc()函数的函数原型为void* malloc(size_t size);，其中size是需要分配的字节数。这个函数返回一个指向所分配内存块起始地址的void*类型指针。
   - free()函数用于释放之前通过malloc()、calloc()或realloc()等函数分配的内存。它的函数原型为void free(void* ptr);，其中ptr是一个指向之前在堆区分配的内存块的有效指针。
   - 堆区

5. - 避免内存泄漏
   - 防止悬空指针
   - 提高内存利用率
   - 优化程序性能

### 二.内存模型的应用
- constValue：常量存储区。
  - *多次输出地址不改变，且添加代码修改constValue会报错。*

- constString：常量存储区。
  - *多次输出地址不改变，且添加代码修改constString会报错。*

- globalVar：全局数据区（静态存储区）
  - *多次输出前七位均为“00007ff”，后四位不变，其余随机变化。如果变量没有存储在全局数据区，当程序执行到某个函数内部时，可能因为其内存被释放变量突然不存在，这会导致无法访问。*

- staticVar：全局数据区（静态存储区）。
  - *多次输出前七位均为“00007ff”，后四位不变，其余随机变化。static用于修饰局部变量，将局部变量声明为静态局部变量，存储在全局数据区（静态存储区）。*

- localVar：栈区。
  - *主函数中无法输出地址，函数中输出地址，前六位恒为“000000”，第十二十三位仍为“ff”，最后一位仍为“4”。当function函数被调用时，localVar在栈上分配内存。当function函数返回时，localVar所占用的内存会被自动释放。如果localVar不是存储在栈区，而是存储在全局数据区，那么每个函数调用都会占用额外的内存空间，导致内存浪费。*

- ptr：栈区。
  - *主函数中无法输出地址，函数中输出地址，前六位恒为“000000”，第十二十三位仍为“ff”，最后一位仍为“0”。它指向的内存是在堆上分配的，但ptr本身是在栈上分配的，假设ptr存储在堆区，那么在函数返回时，就很难自动地回收ptr本身所占用的内存，容易内存泄漏。*

- localVarMain：栈区。
  - *主函数中输出地址，前六位恒为“000000”，第十二十三位仍为“ff”，最后一位仍为“c”当function函数被调用时，localVar在栈上分配内存。作为main函数的局部变量，存储在栈区。当main函数返回时，localVarMain所占用的栈内存会被自动释放。如果它不是存储在栈区，会出现占用全局内存空间问题。*

### 三.浅谈Cache
1.
- ***冯诺伊曼体系结构***是一种计算机设计理念。
一是采用二进制形式表示数据和指令。二是程序存储，即将程序和数据一样存储在存储器中，计算机在运行时可以按照顺序从存储器中取出指令并执行。包含：运算器，控制器，存储器，输入设备，输出设备。
- ***现代计算机组织结构***在冯诺伊曼体系结构的基础上进行了扩展和细化。它仍然保留了冯诺伊曼体系的核心概念，如程序存储和二进制运算，但在各个部件的具体实现和性能优化等方面有了很多发展。
- 不同点：
   - 性能优化方面：
冯诺伊曼体系结构侧重于基本原理的构建，而现代计算机组织结构在性能优化上做了大量工作。
   - 复杂程度方面：
冯诺伊曼体系结构相对简洁，主要描述了计算机的基本组成和工作原理。现代计算机组织结构则更加复杂，由于技术的发展和应用需求的增加，增加了很多新的部件和技术。
   - 存储系统方面：
冯诺伊曼体系结构提出了程序存储的概念，但现代计算机组织结构在存储系统上有了更多的层次和优化。
2.
- 存储单元与地址
- 数据的读写操作
- 与CPU的协同工作
- 存储类型的差异（RAM和ROM）
3.
- Cache（高速缓存）的局部性原理是指：程序在执行过程中，对于存储器的访问呈现出局部性的规律。Cache的设计正是基于这种局部性原理，通过将最可能被频繁访问的内容存储在离CPU更近、速度更快的Cache中，从而提高计算机系统的性能。
- **时间局部性**
  - 时间局部性是指如果一个存储单元被访问，那么在不久的将来，这个存储单元很可能会被再次访问。就好像一个人在短时间内会多次查看同一份文件一样。
- **空间局部性**
  - 空间局部性是指一旦一个存储单元被访问，那么它附近的存储单元也很可能被访问。
4.
- Cache与主存的速度差异
- 基于局部性原理的高效利用
- 减少CPU等待时间
