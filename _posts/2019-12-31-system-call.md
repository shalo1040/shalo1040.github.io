---
layout: post
title: "System Calls"
author: "DAEUN"
---

![structure](/assets/images/operating_system_structure.PNG)
<br><br>
The picture above depicts the structure of operating system services. OS is in the middle of users/programs and hardware. You can see that user can access operating system via user interfaces, such as GUI, bath, and command line interface. All the functionalities that users want to execute have to call system calls and operating system do many jobs such as program execution, I/O operations, accounting, and so on.
<br>
* Unlike MS-DOS implemented all the commands in its CLI, Linux implemented them in a seperate file outside of its CLI. Therefore, Linux's CLI is much more lighter than MS-DOS and it has expandability(확장성).
* Accounting in OS indicates tracked usages of resources.

<br><br><br>
### System Calls
To have services by an operating system, programs must call special functions called system calls. These are usually written in C or C++. Application programmers never see details of system calls, but they can execute system calls according to an API(Application Programming Interface).
<br>
To give you some examples of system call, it is executed frequently in the following situations:
* To **load, execute, create, end, abort** processes and files.
* To **request, release, get/set attributes** of devices.
* To **get/set time or date, get/set system data**.
* To **send and receive** messages, **attach or detach** remote devices, and so on.
<br><br><br>
![system call](/assets/images/system_call.PNG)
<br><br>
The image above shows how the system calls work. If a user application requests to open a file by executing a function in API called _open()_, a system-call interface, a table, finds appropriate index _i_ and executes the actual system call of open() and return again to the user program. This is how system calls work.
<br>
There are several ways to send parameters to OS. First method is to send parameters to registers. Registers, however, are not big enough for storing many parameters. Therefore, there are block and stack method to solve this problem. The block method is to pass parameters to a block or table in memory and pass the address of the block to a register. Similarly, in the stack method a program pushes parameters to a stack and pops them off by the operating system. Usually, Linux and Solaris take the block method for system calls. The illustration of this approach is shown below.
<br>
![block method](/assets/images/block_method.PNG)
<br><br><br>
reference: A. Silberschatz, P.B. Galvin, and G. Gagne, _Operating System Concepts Essentials_, 2nd ed. Hoboken, NJ, USA: Wiley, 2014.