# CPU上下文

CPU 上下文切换

对于多任务操作系统，支持远大于CPU数量的任务同时运行。操作系统通过给不同的任务划分对应时间片，在用户侧看来，多个任务并发运行。而在每一个任务运行前，cpu都需要知道任务从哪里加载、又从哪里开始运行，需要操作系统预先设置好 **cpu寄存器以及程序计数器** 。

# 1. 什么是cpu上下文

CPU 寄存器和程序计数器就是 CPU 上下文，因为它们都是 CPU 在运行任何任务前，必须的依赖环境。

* CPU 寄存器是 CPU 内置的容量小、但速度极快的存储部件，可以用来暂存数据、指令、地址；在cpu的控制部件中有指令寄存器（IR）、程序计数器（PC），cpu 的算术以及逻辑部件中包含累加器（ACC）。
* 程序计数器则是用来存储 CPU 正在执行的指令位置、或者即将执行的下一条指令位置。

# 2. 什么是上下文切换

把上一个任务的CPU上下文保存下来，然后加载下一个任务的cpu上下文，跳转到新任务的程序计数器，执行新的任务。保存下来的cpu上下文会保存在内核中。在任务重新调度执行时再次加载，保证原任务不受影响。

# 3. 上下文切换类型

* 系统调用的上下文切换
* 进程上下文切换
* 线程上下文切换
* 中断上下文切换

## 3.1 系统调用的上下文切换

Linux 根据特权等，将运行空间划分为内核空间以及用户空间。内核空间具有最高权限，可以管理所有资源，用户空间只能管理受限资源，不能直接访问内存资源，必须通过系统调用陷入内核才能访问特定资源。

![](https://pic2.zhimg.com/80/v2-d831951a5e41bbfb1e0e1151a8a2b649_720w.jpg)

kernel-user-ring

由系统调用陷入内核完成从用户态到内核态的转变，这个过程发生的上下文切换：

![](https://pic3.zhimg.com/80/v2-440bb1699b2fa0f0340b38eabcbd7452_720w.jpg)

trap-context

cpu寄存器的用户态指令位置需要先保存下来，然后加载内核态指令位置，最后跳转到内核态运行任务。系统调用结束后，需要重新回到用户态，恢复用户态上下文。一次系统调用发生两次上下文切换。不过，需要注意的是，系统调用过程中，并不会涉及到虚拟内存等进程用户态的资源，也不会切换进程。这跟我们通常所说的进程上下文切换是不一样的（系统调用在同一个进程里运行）。 所以系统调用过程通常称为 特权模式切换 ，而不是上下文切换。但实际上，系统调用过程中，CPU 的上下文切换还是无法避免的。

## 3.1 进程上下文切换

首先进程是由内核管理以及调度的，进程的切换只能发生在内核态。所以进程的上下文切换不仅包括了虚拟内存，栈，全局变量等用户空间的资源，也包括了内核堆栈，寄存器等内核空间的状态。 **因此进程间上下文切换比系统调用多了一步。** 在保存当前进程的内核状态以及CPU寄存器之后，需要保存该进程用户空间的虚拟内存，栈等保存下来；在恢复下一个进程的内核态后，需要恢复下一个进程的虚拟内存以及栈。

**每次上下文切换需要几十纳秒到数微秒之间。** 这个时间还是相当客观的，在高并发场景中，进程下上文切换次数较多，很容易出现大量CPU时间浪费在寄存器，内核栈以及虚拟内存等资源的保存以及恢复上，从而大大减少了进程运行的实际时间。这也是导致系统平均负载升高的原因之一。

 **进程上下文切换的时机** ：

* 进程所分配的时间片耗尽，被系统挂起
* 系统资源不足时，需要等待资源满足才能运行
* 进程调用sleep等主动挂起
* 优先级抢占，更高优先级进程运行，当前进程挂起
* 发生硬件中断时，cpu上的进程被挂起，运行当前中断服务

## 3.2 线程上下文切换

线程与进程的一大区别在于， **线程是调度的基本单位，进程是资源拥有的基本单位** 。内核任务的调度对象是线程，而进程只是给线程提供了虚拟内存、全局变量等资源。线程也拥有自己的数据，比如栈以及寄存器，在上下文切换是也需要保存。线程上下文切换分为两种：

* 不同进程的两个线程，资源不共享，实际上就是进程的上下文切换。
* 同一进程的两个线程，共享虚拟内存，切换时，只需要切换线程的私有数据、寄存器等。

虽然同为上下文切换，但同进程内的线程切换，要比多进程间的切换消耗更少的资源，这也正是多线程代替多进程的一个优势。

## 3.3 中断上下文切换

为了快速响应硬件事件， **中断处理会打断进程的正常调度和运行** ，转而调用中断处理程序，响应设备事件。而在打断其他进程时，需要将其运行状态保存下来，在中断结束后可以恢复原进程的执行。

 **与进程上下文切换不同，中断上下文切换不涉及用户态的操作。** 所以即使一个中断打断了一个正在用户态处理的进程，陷入内核，也不需要保存和恢复进程的虚拟内存、用户栈等。中断上下文只包括内核态发中断服务所需要的状态：包括cpu寄存器、内核堆栈、硬件中断参数等。在中断程序处理结束后，恢复之前进程的内核态上下文，因为用户态的上下文没有被改变，重置PC后可以继续执行。

对同一个 CPU 来说， **中断处理比进程拥有更高的优先级** ，所以中断上下文切换并不会与进程上下文切换同时发生。同样道理，由于中断会打断正常进程的调度和执行，所以大部分中断处理程序都短小精悍，以便尽可能快的执行结束。
