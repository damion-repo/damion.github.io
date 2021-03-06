# 【操作系统】中断


## 中断

中断是为了实现多道程序并发执行，而引入的一种机制

- 中断发生时，`CPU`立即进入核心态
- 当中断发生后，当前运行的进程暂停运行，并由操作系统内核对中断进行处理
- 对于不同的中断信号，会进行不同处理

## 中断的作用

中断可以使`CPU`从用户态切换为核心态，使操作系统获得计算机的控制权。

如果没有中断，cpu将一直运行应用程序，从而无法执行操作系统内核程序。

中断是用户态进入核心态的唯一途径。

核心态到用户态的切换，是通过执行一个特权指令来完成。该指令将程序状态字`PSW`的标志位设置为用户态。

## 中断分类

广义上的中断分为两类：

- 内中断：信号来源于`CPU`内部，与当前执行的指令有关。
- 外中断：信号来源于`CPU`外部，与当前执行的指令无关。

狭义上的中断，就是指外中断，而此时内中断会被称为“异常”。

### 1. 内中断

也称异常、例外。内中断主要有3类：

- 陷阱、陷入（trap)

  由陷入指令引发，此时，应用程序主动将CPU使用权交给操作系统内核。

  系统调用就是通过陷入指令实现。

- 故障（fault)

  由错误条件引起，可能会被内核程序修复。

  内核程序修复故障后，会讲CPU使用权交还给应用程序。

  如：缺页故障。

- 终止（abort)

  由致命错误引起，内核程序通常会直接终止应用程序，不会将CPU使用权归还给应用程序。

  如：整数除0。

### 2. 外中断

每一条指令执行结束，CPU都会检查是否有外部中断信号，如果有，就执行中断处理程序。

常见的外中断，有时钟中断，IO中断请求。

## 中断运行原理

不同的中断信号，需要用不同的中断处理程序来处理。

中断处理程序，一定是内核程序，需要运行在“内核态”。

当CPU检测到中断信号后，会查询中断向量表，找到相应的中断处理程序在内存中的存放位置。中断向量表是由中断信号类型、和中断处理程序指针组成。

