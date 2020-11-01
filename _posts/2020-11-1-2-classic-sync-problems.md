---
layout : post
title : "프로세스 동기화(5) : Classic Problems of Synchronization"
author: "DAEUN"
---

다음 소개하는 세 가지의 고전적 동기화 문제는 새로운 동기화 알고리즘이 개발되었을 때 이를 검증하는 데 쓰인다.

- Bounded-Buffer Problem
- Readers and Writers Problem
- Dining-Philosophers Problem

<br>

## The Bounded-Buffer Problem

![bounded-buffer](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter5/5_0910_ProducerConsumer.jpg)

<br>

- 한정-버퍼 문제
- 초기화
	- int n;
		- 버퍼의 크기
	- semaphore mutex = 1;
		- mutual exclusion
		- lock
	- semaphore empty = n;
		- 비어있는 버퍼의 개수: n
	- semaphore full = 0;
		- 차있는 버퍼의 개수: 0
- wait()는 감소, signal()은 증가시킴
- producer
	- 비어있는 버퍼의 수는 감소, 차있는 버퍼의 수는 증가
	- wait(empty)
		- empty--
	- signal(full)
		- full++
- consumer
	- 차있는 버퍼의 수는 감소, 비어있는 버퍼의 수는 증가
	- wait(full)
		- full--
	- signal(empty)
		- empty++

<details>
	<summary> wait(), signal() 코드 </summary>
	
![code](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter5/5_Semaphore2.jpg)

</details>

<br><br>

## The Readers-Writers Problem

- readers: read only
- writers: read and write

- 여러 readers가 동시에 공유자원을 읽는 것은 문제가 되지 않음
- reader-writer 또는 writer-writer가 함께 접근하면 문제가 될 수 있음<br>
=> writer는 단독으로 사용하도록 제어해야 함
=> reader 또는 writer에게 우선순위 부여하는 형태로 해결

- reader에게 우선순위 부여
	- writer가 자원에 접근하고 있지 않다면, reader는 언제나 자원에 접근 가능
	- 즉, reader가 자원을 읽고 있을 때, writer가 기다리고 있더라도 중단하지 않음<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=> writer의 기아(starvation) 문제 발생

<br>

- writer에게 우선순위 부여
	- writer가 ready 상태가 되면 곧바로 자원 접근 가능
	- 즉, writer가 자원에 접근하기를 기다리고 있는 상태라면, 다른 새로운 reader는 자원 접근 불가<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=> reader의 기아(starvation) 문제 발생

<br><br>

다음은 reader의 우선순위가 높은 reader-writer 문제에서의 동기화를 위한 코드이다.

<br>

![reader-writer](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter5/5_1112_ReaderWriterStructures.jpg)

<br>

- 초기화
	- semaphore rw_mutex = 1;
		- writer의 자원 접근 시, 상호 배제를 위한 세마포
		- 처음으로 reader가 접근할 때, 마지막 reader가 임계구역을 나올 때도 사용
		- 이미 reader가 읽고 있을 때 접근하는 reader는 사용하지 않음
	- semaphore mutex = 1;
		- read_count의 갱신 시, 상호 배제를 위한 세마포
	- int read_count = 0;
		- 현재 자원을 읽고 있는 reader의 수

- readers-writers lock
	- readers-writers 문제와 해결 방법이 일반화되어 readers-writers lock을 제공하기도 함
	- reader 또는 writer 모드
	- 유용하게 사용되는 경우
		- 어떤 프로세스가 공유 자원을 only read 또는 only write하는지 쉽게 식별할 수 있는 경우
		- reader의 수가 writer보다 많을 경우
			- reader-writer lock을 만드는 것은 세마포나 mutex 락보다 오버헤드가 크기 때문

<br>

## The Dining-Philosophers Problem

<br>

<img src="https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter5/5_13_DiningPhilosophers.jpg" width="350">

<br>

- 식사하는 철학자 문제
- 문제
	- 일생을 think와 eat만 하는 5명의 철학자가 원형 탁자에 둘러 앉아 있다.
	- 탁자의 중앙에는 음식이 놓여 있고, 철학자의 양 옆에는 젓가락이 하나씩, 총 다섯 개 놓여 있다.
	- 철학자는 배가 고프면 자신의 양 옆에 놓인 두 젓가락을 들고 음식을 먹는다. 이 때, 젓가락을 한 번에 하나만 들 수 있다.
	- 음식을 먹은 후 다시 두 젓가락을 원래 자리에 내려 놓는다.
- large class of concurrency-control problem
- deadlock과 starvation 없이 문제를 해결해야한다.

<br>

<img src="https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter5/5_14_Philosopher_i.jpg" width="400">

<br>

- 해결 방법
	- A. 다섯 개의 젓가락을 세마포로 구현하고 각각의 젓가락을 얻을 때는 wait(), 반납할 때는 signal() 호출
		- 동시에 서로 다른 철학자가 같은 젓가락을 얻지 못하는 상호 배제는 만족
		- 하지만, **deadlock** 발생 가능
			- 모든 철학자가 동시에 왼쪽 젓가락을 집는 경우, 오른쪽에 있는 젓가락을 집은 철학자가 이를 반납할 때까지 기다릴 것임. 하지만 모든 철학자가 같은 상황이므로 영원히 deadlock한다.
	- A를 보완하기 위한 방법
		- B. 탁자에 4명의 철학자만 앉힌다.
		- C. 양쪽의 젓가락이 모두 집을 수 있는 상태인지 확인하고 임계구역에서 젓가락을 얻는다.
		- D. 홀수번째 철학자는 왼쪽 젓가락 먼저, 짝수번째 철학자는 오른쪽 젓가락 먼저 집은 후 반대쪽 젓가락을 집도록한다. (비대칭적, asymmetric solution)
	- 주의: **데드락이 없는 방법이라고 하더라도 기아 발생이 없어지는 것은 아니다.**

<br><br>

reference: A. Silberschatz, P.B. Galvin, and G. Gagne, _Operating System Concepts Essentials_, 2nd ed. Hoboken, NJ, USA: Wiley, 2014.