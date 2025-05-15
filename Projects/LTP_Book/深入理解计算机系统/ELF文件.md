# ELF文件

> ELF文件（Executable Linkable Format）是一种文件存储格式。Linux下的目标文件和可执行文件都按照该格式进行存储，有必要做个总结。

* [1. 链接举例](https://zhuanlan.zhihu.com/write)
* [2. ELF文件类型](https://zhuanlan.zhihu.com/write)

  * [2.1 可重定位目标文件（.o文件）](https://zhuanlan.zhihu.com/write)
  * [2.2 可执行目标文件（a.out文件）](https://zhuanlan.zhihu.com/write)
  * [2.3 共享对象文件（.so文件）](https://zhuanlan.zhihu.com/write)
* [3. ELF文件作用](https://zhuanlan.zhihu.com/write)
* [4. ELF文件格式](https://zhuanlan.zhihu.com/write)

  * [4.1 从编译和链接角度看ELF文件（可重定位目标文件）](https://zhuanlan.zhihu.com/write)
  * [4.2 从程序执行角度看ELF文件（可执行文件）](https://zhuanlan.zhihu.com/write)
* [5.总结](https://zhuanlan.zhihu.com/write)

## **1. 链接举例**

在介绍ELF文件之前，我们先看下，一个.c程序是如何变成可执行目标文件的。下面举个例子。

该程序由main.c和sum.c两个模块组成。sum.c接收数组和数组长度两个参数，最后将数组求和的结果返回。main.c调用sum函数，并传递一个两元素的int数组array，将计算结果保存在val中。

```text
//main.c
int sum(int *a, int n);

int array[2] = {1, 2};

int main(int argc, char** argv)
{
    int val = sum(array, 2);
    return val;
}

//sum.c
int sum(int *a, int n)
{
    int i, s = 0;

    for (i = 0; i < n; i++) {
        s += a[i];
    }
    return s;
}
```

让我们来看看如果我们使用GCC编译两个模块会发生什么？

main.c和sum.c将分别通过**翻译器**将源文件处理为可重定位的目标文件main.o和sum.o。翻译器处理的过程包括了 **预处理（ccp）** 、 **编译（ccl）** 、**汇编（as）** 三个过程。最后，**链接器（ld）** 将可重定位的目标文件main.o和sum.o以及一些必要的系统文件组合起来，创建一个可执行目标文件prog。具体过程如下图所示。

![](https://pic2.zhimg.com/80/v2-3dbd384c431e5c0c3075ae2c5d6be701_1440w.jpg)

由上面的过程，我们可以看出在经过汇编器后会输出一个.o文件，这个叫做 **可重定位的目标文件** 。将main.o和sum.o输入链接器后，链接器输出的prog文件叫做 **可执行目标文件** 。那这两个目标文件有什么样的区别呢？

## **2. ELF文件类型**

### **2.1 可重定位目标文件（.o文件）**

包含二进制代码和数据， **其形式可以和其他目标文件进行合并** ，创建一个可执行目标文件。例如lib*.o文件。

### **2.2 可执行目标文件（a.out文件）**

包含二进制代码和数据， **可直接被加载器加载执行** 。例如编译好的可执行文件a.out。

### **2.3 共享对象文件（.so文件）**

用于和其他共享目标文件或者可重定位文件一起生成ELF目标文件或者和执行文件一起创建进程映像，例如lib*.so文件。

## **3. ELF文件作用**

ELF文件参与程序的连接(建立一个程序)和程序的执行(运行一个程序)，所以可以从**不同的角度**来看待ELF格式的文件：

1.如果用于 **编译和链接（可重定位文件）** ，则编译器和链接器将把ELF文件看作是节头表描述的节的集合,程序头表可选。

2.如果用于 **加载执行（可执行文件）** ，则加载器则将把ELF文件看作是程序头表描述的段的集合，一个段可能包含多个节，节头表可选。

## **4. ELF文件格式**

### **4.1 从编译和链接角度看ELF文件（可重定位目标文件）**

![](https://pic1.zhimg.com/80/v2-8f3c928c248df890fdafec9637c85030_1440w.jpg)

**ELF头**

每个ELF文件都必须存在一个ELF_He ader,这里存放了很多重要的信息用来 **描述整个文件的组织** ,如: 版本信息,入口信息,偏移信息等。程序执行也必须依靠其提供的信息。

**段头表**

段头表。存放的是所有 **不同段将在内存中的位置** 。

**.text section**

代码段。存放已编译程序的机器代码，一般是只读的。

**.rodata** **section**

只读数据段。此段的数据不可修改,存放常量。比如，printf中的格式化语句。

**.data** **section**

数据段。存放已初始化的全局变量、常量。

**.bss** **section**

bss段。未初始化全局变量，仅是占位符， **不占据任何实际磁盘空间** 。目标文件格式区分初始化和非初始化是为了空间效率。

![](https://pic1.zhimg.com/80/v2-23d5e140883aaf3f1df2649962a4c6a8_1440w.jpg)

**.symtab** **section**

符号表，它存放在程序中定义和引用的函数和全局变量的信息。

**.rel.txt** **section**

.text节的重定位信息，用于重新修改代码段的指令中的地址信息。

**.rel.data** **section**

.data节的重定位信息，用于对被模块使用或定义的全局变量进行重定位的信息。

**.debug** **section**

调试用的符号表。

**.strtab section**

包含 symtab和 debug节中符号及节名。

**节头部表**

每个节的节名、偏移和大小。

以下是32位系统对应的节头表数据结构,说明了每个节的节名、在文件中的偏移、大小、访问属性、对齐方式等。

```text
typedef struct {
    Elf32_Word sh_name;   //节名字符串在.strtab节（字符串表）中的偏移
    Elf32_Word sh_type;   //节类型：无效/代码或数据/符号/字符串/...
    Elf32_Word sh_flags;  //节标志：该节在虚拟空间中的访问属性
    Elf32_Addr sh_addr;   //虚拟地址：若可被加载，则对应虚拟地址
    Elf32_Off  sh_offset; //在文件中的偏移地址，对.bss节而言则无意义
    Elf32_Word sh_size;   //节在文件中所占的长度
    Elf32_Word sh_link;   //sh_link和sh_info用于与链接相关的节（如 .rel.text节、.rel.data节、.symtab节等）
    Elf32_Word sh_info;
    Elf32_Word sh_addralign; //节的对齐要求
    Elf32_Word sh_entsize;   //节中每个表项的长度，0表示无固定长度表项
} Elf32_Shdr;
```

使用readelf命令命令查看节头表内容

```text
[ubuntu@localhost interpositioning]$ readelf -S main.o
There are 13 section headers, starting at offset 0x3f8:

Section Headers:
  [Nr] Name              Type             Address           Offset
       Size              EntSize          Flags  Link  Info  Align
  [ 0]                   NULL             0000000000000000  00000000
       0000000000000000  0000000000000000           0     0     0
  [ 1] .text             PROGBITS         0000000000000000  00000040
       0000000000000071  0000000000000000  AX       0     0     1
  [ 2] .rela.text        RELA             0000000000000000  000002d0
       0000000000000090  0000000000000018   I      11     1     8
  [ 3] .data             PROGBITS         0000000000000000  000000b1
       0000000000000049  0000000000000000  WA       0     0     1
  [ 4] .bss              NOBITS           0000000000000000  000000b1
       000000000000000c  0000000000000000  WA       0     0     1
  [ 5] .rodata           PROGBITS         0000000000000000  000000b1
       0000000000000019  0000000000000000   A       0     0     1
  [ 6] .comment          PROGBITS         0000000000000000  000000ca
       0000000000000035  0000000000000001  MS       0     0     1
  [ 7] .note.GNU-stack   PROGBITS         0000000000000000  000000ff
       0000000000000000  0000000000000000           0     0     1
  [ 8] .eh_frame         PROGBITS         0000000000000000  00000100
       0000000000000058  0000000000000000   A       0     0     8
  [ 9] .rela.eh_frame    RELA             0000000000000000  00000360
       0000000000000030  0000000000000018   I      11     8     8
  [10] .shstrtab         STRTAB           0000000000000000  00000390
       0000000000000061  0000000000000000           0     0     1
  [11] .symtab           SYMTAB           0000000000000000  00000158
       0000000000000150  0000000000000018          12     9     8
  [12] .strtab           STRTAB           0000000000000000  000002a8
       0000000000000023  0000000000000000           0     0     1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings), l (large)
  I (info), L (link order), G (group), T (TLS), E (exclude), x (unknown)
  O (extra OS processing required) o (OS specific), p (processor specific)
```

可重定位目标文件中，每个可装入节的起始地址总是0。

.bss节应占0x0c大小，但只有装入内存时才会分配。

### **4.2 从程序执行角度看ELF文件（可执行文件）**

![](https://pic4.zhimg.com/80/v2-452142a78d83bb9d90fe9055cd791d77_1440w.jpg)

与可重定位目标文件不同：

1.ELF头中字段 e_entry给出执行程序时第一条指令的地址，而在可重定位文件中，此字段为0。

2.多一个init节，用于定义init函数，该函数用来进行可执行目标文件开始执行时的初始化工作。

3.少两.rel节（无需重定位)。

4.多一个程序头表，也称段头表，是一个结构数组。

使用readelf命令查看ELF头的内容：

```text
[ubuntu@localhost interpositioning]$readelf -h main.o
ELF Header:
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00
  Class:                             ELF64
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              REL (Relocatable file)
  Machine:                           Advanced Micro Devices X86-64
  Version:                           0x1
  Entry point address:               0x0
  Start of program headers:          0 (bytes into file)
  Start of section headers:          1064 (bytes into file)
  Flags:                             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           32 (bytes)         //程序头表每项32B
  Number of program headers:         8                  //程序头表共8项
  Size of section headers:           64 (bytes)
  Number of section headers:         13
  Section header string table index: 10                //.strtab在节头表中的索引
```

装入内存时，ELF头、程序头表、.init节、.rodata节会被装入只读代码段。.data节和.bss节会被装入读写数据段。

 **段头表能够描述可执行文件中的节与虚拟空间中的存储段之间的映射关系** 。一个表项32B，说明虚拟地址空间中一个连续的片段或一个特殊的节。以下是32位系统对应的段头表数据结构：

```text
typedef struct {
    Elf32_Word p_type;   //此数组元素描述的段的类型，或者如何解释此数组元素的信息。
    Elf32_Off p_offset;  //此成员给出从文件头到该段第一个字节的偏移
    Elf32_Addr p_vaddr;  //此成员给出段的第一个字节将被放到内存中的虚拟地址
    Elf32_Addr p_paddr;  //此成员仅用于与物理地址相关的系统中。System V忽略所有应用程序的物理地址信息。
    Elf32_Word p_filesz; //此成员给出段在文件映像中所占的字节数。可以为0。
    Elf32_Word p_memsz;  //此成员给出段在内存映像中占用的字节数。可以为0。
    Elf32_Word p_flags;  //此成员给出与段相关的标志。
    Elf32_Word p_align;  //此成员给出段在文件中和内存中如何对齐。
} Elf32_phdr;
```

使用readelf命令查看某可执行目标文件的程序头表。

```text
[ubuntu@localhost interpositioning]$readelf -l main

Elf file type is EXEC (Executable file)
Entry point 0x400550
There are 9 program headers, starting at offset 64

Program Headers:
  Type           Offset             VirtAddr           PhysAddr
                 FileSiz            MemSiz              Flags  Align
  PHDR           0x0000000000000040 0x0000000000400040 0x0000000000400040
                 0x00000000000001f8 0x00000000000001f8  R E    8
  INTERP         0x0000000000000238 0x0000000000400238 0x0000000000400238
                 0x000000000000001c 0x000000000000001c  R      1
      [Requesting program interpreter: /lib64/ld-linux-x86-64.so.2]
  LOAD           0x0000000000000000 0x0000000000400000 0x0000000000400000
                 0x00000000000008ac 0x00000000000008ac  R E    200000
  LOAD           0x0000000000000e10 0x0000000000600e10 0x0000000000600e10
                 0x0000000000000240 0x0000000000000248  RW     200000
  DYNAMIC        0x0000000000000e28 0x0000000000600e28 0x0000000000600e28
                 0x00000000000001d0 0x00000000000001d0  RW     8
  NOTE           0x0000000000000254 0x0000000000400254 0x0000000000400254
                 0x0000000000000044 0x0000000000000044  R      4
  GNU_EH_FRAME   0x0000000000000780 0x0000000000400780 0x0000000000400780
                 0x0000000000000034 0x0000000000000034  R      4
  GNU_STACK      0x0000000000000000 0x0000000000000000 0x0000000000000000
                 0x0000000000000000 0x0000000000000000  RW     10
  GNU_RELRO      0x0000000000000e10 0x0000000000600e10 0x0000000000600e10
                 0x00000000000001f0 0x00000000000001f0  R      1
```

程序头表信息有9个表项，其中两个为可装入段(即Type=LOAD)：

第一可装入段(第15,16行)：第0x0000 0~0x0x8ab的长度为0x8ac字节的ELF头、程序头表、.init、.text和.rodata节，映射到虚拟地址0x400000开始长度为0x8ac字节的区域 ，按0x200000=2MB对齐，具有只读/执行权限（Flg=RE），是只读代码段。

第二可装入段(第17,18行)：第0xe10 ~0x104f的长度为0x240字节的.data节和磁盘中不占存储空间的.bss节，映射到虚拟地址0x600e10开始长度为0x248字节的存储区域，在0x248=584B存储区中，前0x240= 576B用.data节内容初始化，后面584-576= 8B对应.bss节，初始化为0 ，按0x200000 =2MB对齐，具有可读可写权限(Flg=RW)，是可读写数据段。

 **由此看出.bss节在文件中不占用磁盘空间，但在存储器中需要给它分配相应大小的空间** 。

## **5.总结**

1.链接处理涉及到三种目标文件格式： **可重定位目标文件、可执行目标文件和共享目标文件** 。共享库文件是一种特殊的可重定位目标。

2.ELF目标文件格式可以从**编译链接角度**和**程序执行角度**两个角度看，前者是可重定位目标格式，后者是可执行目标格式。从编译链接角度看，可重定位目标文件中包含ELF头、各个节以及节头表。可执行目标文件中包含ELF头、程序头表（段头表）以及各种节组成的段。

3.bss段在可执行目标文件中不会有它的空间，只有当可执行目标文件 **装载运行时** ，才会被分配内存（并且位于data段内存块之后）， **并且初始化为0** 。

> 本文参考  
> 《深入理解计算机系统》

‍

‍

‍

ELF 文件

# 1. 基本问题

Linux命令行运行一个可执行文件，这个过程发生了什么？可执行文件以什么格式进行组织？操作系统如何加载可执行文件，创建进程，调度运行？

首先需要了解，计算机的操作系统的启动引导程序写在特定存储空间，计算机上电后自动加载启动程序，开启主进程，之后的所有进程都是不断fork主进程得到的。对于可执行文件，也是由shell进程fork一个子进程，然后进行一系列系统调用装载亏歘看文件并执行。

不同系统的可执行文件有不同的格式，**ELF (Executable and Linkable Format)** 是Unix系统实验室作为应用程序二进制接口 **（ABI）** 而发布的可执行文件格式.

ELF文件主要有四种不同类型：

* **可重定位文件（Relocatable File）** ： 包含与其他文件链接来创建可执行文件或者共享目标文件的数据，`xxx.o` 文件
* **可执行文件（Executable File）** ：包含用于执行的程序的文件，此文件规定了`exec（）` 如何创建一个程序的进程映像， `a.out` 文件
* **共享目标文件（Shared Object File）** : 包含可在两种上下文中链接的代码与数据：

  * 链接编译器可以将它与其他可重定位文件和共享目标文件一起处理，生成另外一个目标文件`*.so`
  * 动态链接器`Dynamic Linker` 可能将其与一个可执行文件、其他`so`文件一起创建程序进程映像
* **内核转储（Core Dumps）** ： 存放当前进程的执行上下文，用于Dump信号触发。

# 2. 加载与动态链接

从编译/链接和运行的角度看，库分为动态链接和静态链接。相应的两种不同的ELF格式映像：

* 一种是静态链接的，在装入/启动其运行时无需装入函数库映像、也无需进行动态连接。
* 另一种是动态连接，需要在装入/启动其运行时同时装入函数库映像并进行动态链接。

Linux内核既支持静态链接的ELF映像，也支持动态链接的ELF映像，GNU规定：

* 把ELF映像的装入/启动入在Linux内核中；
* 把动态链接的实现放在用户空间（glibc），并为此提供一个称为”解释器”(ld-linux.so.2)的工具软件，而解释器的装入/启动也由内核负责。

# 3. ELF加载过程

## 3.1 `execve()`

在shell中执行命令后，shell获取桥入的指令，并执行`execve()` 函数，该函数接受可执行文件名以及程序所需参数，同时传入环境变量。 **负责对于进程栈进行初始化，压栈环境变量值，压栈参数，压栈可执行文件名。** 初始化完成后，调用`sys_execuve()`陷入内核, 调用链路如下：

1

sys_execve / sys_execveat

2

└→ do_execve

3

      └→ do_execveat_common

4

       └→ __do_execve_file

5

            └→ exec_binprm

6

            └→ search_binary_handler

7

                         └→ load_elf_binary

Copied!

## 3.2 `sys_execve()`

该函数进行一些参数的检查与复制，而后调用 `do_execve()`

## 3.3 `do_execve`

内核中实际执行`execv()`或`execve()`系统调用的程序是`do_execve()`，这个函数先打开目标映像文件，并从目标文件的头部（第一个字节开始）读入若干（├─systemd─┬─当前Linux内核中是128）字节（实际上就是填充ELF文件头）， 读取文件头后可以判断文件格式，进而调用`serach_binary_handler()`

## 3.4 `search_bianry_handler()`

该函数将去搜索和匹配合适的可执行文件装载处理程序。Linux 中所有被支持的可执行文件格式都有相应的装在处理程序。以Linux 中的ELF 文件为例，接下来将会调用elf 文件的处理程序：`load_elf_binary()`

## 3.5 `load_elf_binary()`

该函数执行以下三个步骤:

a）创建虚拟地址空间：实际上指的是建立从虚拟地址空间到物理内存的映射函数所需要的相应的数据结构。（即创建一个空的页表）

b）读取可执行文件的文件头，建立可执行文件到虚拟地址空间之间的映射关系

c）将CPU指令寄存器设置为可执行文件入口（虚拟空间中的一个地址）

`load_elf_binary()`函数执行完毕，事实上装载函数执行完毕后，可执行文件真正的指令和数据都没有被装入内存中，只是建立了可执行文件与虚拟内存之间的映射关系，以及分配了一个空的页表，用来存储虚拟内存与物理内存之间的映射关系。

## 3.6 程序返回到`execve()`中

此时从内核态返回到用户态，且寄存器的地址被设置为了ELF 的入口地址，于是新的程序开始启动，发现程序入口对应的页面并没有加载（因为初始时是空页面），则此时引发一个缺页错误，操作系统根据可执行文件和虚拟内存之间的映射关系，在磁盘上找到缺的页，并申请物理内存，将其加载到物理内存中，并在页表中填入该虚拟内存页与物理内存页之间的映射关系。之后程序正常运行，直至结束后回到shell 父进程中，结束回到 shell。
