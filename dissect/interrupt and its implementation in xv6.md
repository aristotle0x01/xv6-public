# 基于xv6的中断原理及实现

## 1.操作系统为什么需要中断

​        中断作为一种设计其本质是为了提升cpu利用率，避免忙等待。譬如访问磁盘，网卡等，可以不必频繁询问结果，而由IO完成时自行通知cpu。

​        **忙等待**<sub>1</sub>：

![](https://user-images.githubusercontent.com/2216435/232430844-6d6335ce-5055-4d19-9ea2-a511fabe0751.png)

​         **中断**<sub>1</sub>：

![](https://user-images.githubusercontent.com/2216435/232431319-259b7a91-cee9-44bf-879d-e5e1549aae2e.png)



## 2.中断源

一般分为两种：

- 硬件中断
- 软件中断

**8086管脚图**<sub>2</sub>，其中INTR & NMI即为两个中断相关pin：

![](https://user-images.githubusercontent.com/2216435/232446526-eb2e5ba5-dacb-4f2b-a259-3992709096cb.png)

​        对于硬件中断，当local APIC处于激活状态时，INTR & NMI接入APIC来统一管理中断。未激活时则INTR & NMI直接通知cpu。

​       软件中断则由int指令产生。

​       中断码表：

![](https://user-images.githubusercontent.com/2216435/232666432-bf7b4a76-a01b-4e82-84b6-2190761a4a7c.png)

## 3.屏蔽(maskable)问题

​       NMI类中断不可屏蔽，意味着如果当前中断未执行完毕，后续中断不可被处理。而其它中断则可以根据EFLAGS上的IF&TF标记进行屏蔽。

​       所以，屏蔽的对象是当前中断，而不是潜在的后续中断。意味着当前中断不可屏蔽忽视，此时不能处理其它中断。

## 4.idt & gdt的关系

​        xv6启动时，会执行lgdt & lidt指令初始化IDTR & GDTR寄存器，以支持中断时的处理程序跳转<sub>3</sub>

![](https://user-images.githubusercontent.com/2216435/232688625-f75c06a4-1e70-4d09-8970-801c6324d728.png)



![](https://user-images.githubusercontent.com/2216435/232688899-96c5614d-584a-47f2-8eca-5ff10a1f6977.png)



## 3.中断时栈的变化



## 4.中断的作用





## Ref

1.[OS02: Interrupts and I/O](https://oer.gitlab.io/OS/Operating-Systems-Interrupts.pdf) 

2.[Pin diagram of 8086 microprocessor](https://www.geeksforgeeks.org/pin-diagram-8086-microprocessor/)

3.[IA-32 Intel® Architecture Software Developer’s Manual Volume 3: System Programming Guide CHAPTER 5 INTERRUPT AND EXCEPTION HANDLING](http://flint.cs.yale.edu/cs422/doc/24547212.pdf)

