---
layout: post
title: "What is Operating System? (1)"
author: "DAEUN"
---

### What is OS?
There is no perfect definition for Operating Systems. However, we need something that can manage between hardware and software application so that they can safely operate their job. For example, operating systems make processes use hardware resources such as CPU, memory space, and I/O efficiently. We call this **resource allocation(자원할당)** and **resource utilization(자원이용)**. Also, OS is the one program which is called **kernel**, running all the time while computer is turned on.

### Computer System Organization
Since CPU and device controllers access to shared memory via a common bus, a memory controller should make sure that they all orderly access to memory.

### How does a computer start running?
![boot](/assets/images/os_boot.PNG)
1. When the power is on, **bootstrap** is the first program that executes in **ROM** or in **EEPROM**.
2. The bootstrap program load operating system kernel from disk to memory.
3. CPU executes kernel and start services both to system and users.

Operating system runs as an event-driven, also called interrupt-driven. As you just saw above, computer boots by event-driven. Also, when a user writes text through keyboard and hits enter, the CPU is interrupted so that it stops what it was doing and starts processing the output of I/O interrupt. Hardware can trigger interrupt by sending a signal to the CPU, and software can trigger interrupt by executing operation so called **system call**.

When an interrupt occurs, it saves it's current state into PCB in order to come back and resume the process later. Then it accesses to **Interrupt Vector(IV)** which knows the location of **interrupt service routine(처리 방법 명시 함수)** that specifies what to do. After the CPU finishes operating interrupt service routine, it goes back to previous process and resume remained jobs.

### Computer System Architecture
If there exists only one CPU, then we call its system a **Single-Processer System**. By having special-purpose processors, it helps relieving overhead of CPU. **Multiprocessor System** has two or more processors and it has three advantages. First, since there are more processers, it has increased throughput. Second, it has advantage in economy of scale. For instance, we needed 10 computers, keyboards, and mouses in single-processer system, but now a multiprocessor system with 10 processers can do the same job, which results in less hardware devices. Last, it has increased reliability. If processers runs in parallel and one of those is broken, other processers are still working so that the system doesn't halts. We call this ability **graceful degradation(우아한 퇴보)** which we also call this **fault tolerence(결함허용)**.

reference: A. Silberschatz, P.B. Galvin, and G. Gagne, _Operating System Concepts Essentials_, 2nd ed. Hoboken, NJ, USA: Wiley, 2014.