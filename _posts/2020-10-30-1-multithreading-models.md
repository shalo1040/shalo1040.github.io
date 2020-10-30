---
layout : post
title : "Multithreading Models"
author : "DAEUN"
---

### Many-to-One

<img src="https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter4/4_05_ManyToOne.jpg" width="400">

<br>

- 다수의 사용자 수준 스레드가 커널 스레드에 매핑
- (-) 한 스레드가 블록되면 모든 스레드도 블록
- (-) 여러 스레드가 병렬로 실행될 수 없음
	- 하나의 사용자 스레드만이 커널에 존재할 수 있음
- 거의 사용하고 있지 않는 모델

<br>

### One-to-One

<img src="https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter4/4_06_OneToOne.jpg" width="500">

<br>

- 사용자 스레드와 커널 스레드 간 1:1 매핑
- (+) 병렬 수행 가능
- (-) 사용자 스레드를 생성하면 커널 스레드도 생성
	- 너무 많이 생성하면 오버헤드 발생<br>
	=> 스레드의 개수 제한

<br>

### Many-to-Many

![many-to-many](https://i2.wp.com/zitoc.com/wp-content/uploads/2019/02/Multithreading.png?fit=500%2C453&ssl=1)

<br>

- 여러 사용자 수준 스레드를 그보다 작거나 같은 수의 커널 수준 스레드로 매핑
- (+) 병렬 수행 가능
- (+) 필요한만큼 스레드 생성 가능
- (+) 하나의 스레드가 블록되면 커널은 다른 스레드를 스케줄함

<br><br>

reference: A. Silberschatz, P.B. Galvin, and G. Gagne, _Operating System Concepts Essentials_, 2nd ed. Hoboken, NJ, USA: Wiley, 2014.