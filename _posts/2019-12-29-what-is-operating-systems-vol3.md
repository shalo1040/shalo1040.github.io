---
layout: post
title: "What is Operating System? (3)"
author: "DAEUN"
---

### Protection and Security
Protection is a mechanism for controlling access of processes or users to resources. On the other hand, security defends the system from external or internal attacks like viruses and worms.

### Data Structures in Operating Systems
Main memory is constructed as an array, calling and returning function is available by stack, and queue is used to make waiting list for processes that requests to use printers or CPU. In Linux, balanced binary search tree is used in CPU-scheduling. The system checks if the user id and password is correct by using hash function. When the user enters his/her id and password, its id checks the matched hash map and checks if the input password is the same as the value in hash map. In addition, bitmap is used to check if the resources is available or not.
<br><br><br>
reference: A. Silberschatz, P.B. Galvin, and G. Gagne, _Operating System Concepts Essentials_, 2nd ed. Hoboken, NJ, USA: Wiley, 2014.