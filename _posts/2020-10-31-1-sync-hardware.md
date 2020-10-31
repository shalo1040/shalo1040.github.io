---
layout : post
title: "프로세스 동기화(2) : Hardware"
author: "DAEUN"
---

## locking

lock이 허용됐을 때만 임계구역에 들어갈 수 있다는 아이디어.

- 단일처리기 환경(single-processor)에서는 critical-section 문제 해결이 간단하다.
	- 공유 자원이 변경되는 동안 인터럽트 발생을 허용하지 않는다.
		- 비선점형 커널
	- But, 다중처리기 환경에서는 비효율적
		- 모든 처리기에 인터럽트를 허용하지 않는 massage passing -> 시간 지체
		- 인터럽트로 clock이 업데이트 -> 시간 지체

=> 원자적(automically)으로 critical-section 문제를 해결하는 하드웨어 명령 제공<br>
&nbsp;&nbsp;&nbsp; - test_and_set(), compare_and_swap()<br>

* 원자적 : 인터럽트에 의해 중단될 수 없는 성질을 갖고 있음

<br>

## test_and_set()

![code1](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter5/5_0304_TestAndSet.jpg)

<br>

- 원자적 실행
- 전달된 매개변수의 현재 값 반환
- 전달된 매개변수의 값에 TRUE 대입
- Boolean형 공유 변수 lock은 FALSE로 초기화

<br>

## compare_and_swap()

![code2](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter5/5_0506_CompareAndSwap.jpg)

<br>

- 원자적 실행
- 전달된 매개변수 value의 현재 값 반환
- value == expected일 경우에만 임계구역 진입 가능
- 정수형 공유 변수 lock은 0으로 초기화

<br>

## 한정된 대기 만족 X

- 앞서 살펴본 코드는 상호 배제는 만족하지만, 한정된 대기를 만족하지 않음
	- exit section에서 다음과 같이 처리를 해주어야 함

![code3](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter5/5_07_TestAndSet2.jpg)

<br>

- 상호 배제 조건 만족
	- (초기 상태를 제외하고) 오직 다른 프로세스가 임계구역을 빠져나왔을 때, waiting[i]가 false가 됨
	- 오직 하나의 waiting[i]만 false

<br>

- 진행 조건 만족
	- exit section에서 lock 또는 waiting[j]를 false로 변경
		- 이는 임계구역에 진입하려 대기하는 프로세스를 진입할 수 있게 함

<br>

- 한정된 대기 만족
	- exit section에서 waiting을 순차적으로 탐색하며 가장 첫번째로 대기 중(waiting[j] == true)인 프로세스가 임계구역에 진입할 수 있게 됨
	- 최대 n-1번 기다리면 임계구역 진입할 수 있음이 보장

<br><br>

reference: A. Silberschatz, P.B. Galvin, and G. Gagne, _Operating System Concepts Essentials_, 2nd ed. Hoboken, NJ, USA: Wiley, 2014.