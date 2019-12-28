---
layout: post
title: "What is Operating System? (2)"
author: "DAEUN"
---

In Multiprogramming system, CPU executes one job in main memory and if it needs I/O operations, the process would wait I/O operations. In non-multitasking system, the CPU will sit idle at this stage. In multiprogramming system, however, the operating system switches to other jobs in the memory so that never let the CPU sit idle unless there is no job to execute. As you can see here, the multiprogramming system uses system resources such as CPU, memory more efficiently, but actually it doesn't interact with users.
<br>
There is another system called time sharing(multitasking). This allows the CPU to execute jobs in the memory alternatively so that it seems that multiple programs are running at the same time and interacting with users despite it isn't.
<br>
To make the above system, we need CPU scheduling and multiprogramming. We're going to study **CPU scheduling**, which determines which job to execute first, **memory management**, which loads jobs from job pool(disk) to the main memory, **process synchronization**, which manages process to correctly read and write values, and so on. Also, we will learn **swapping** and **virtual memory** which can reduce the response time of the CPU.
<br>
### User Mode and Kernel Mode
We can distinguish between which program code is executing by differentiating **mode bit** between them. Mode bit identifies current mode: kernel mode(0) or user mode(1)
When the system is executing in user mode and user wants to use a service from OS via a system call, the mode bit switches to kernel mode, which is 0, and after the system finishes operations, it switches back to user mode, which is 1.
There are privileged instructions that only could be executed in kernel mode in order to protect OS from errant users. For example, switching kernel mode from user mode is one of privileged instructions.

reference: A. Silberschatz, P.B. Galvin, and G. Gagne, _Operating System Concepts Essentials_, 2nd ed. Hoboken, NJ, USA: Wiley, 2014.