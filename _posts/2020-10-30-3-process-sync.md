---
layout : post
title : "프로세스 동기화(1) : 임계구역, Peterson's Solution"
author : "DAEUN"
---

## 경쟁 상황(race condition)

여러 프로세스가 같은 공유 자원을 동시에 수정하려 하는 경우, 데이터가 잘못되는 문제가 발생할 수 있다. 이러한 상황을 경쟁 상황이라고 한다.

- 생산자 소비자 문제
	- buffer의 공간을 모두 활용하기 위해 몇 개의 공간이 찼는지를 세는 counter 변수를 둘 수 있다. But..
	- counter++, counter--
		- 각각 저급 언어로 3개의 일처리 필요
			1. register = counter
			2. register = register + 1
			3. counter = register
		- 병렬 또는 병행 실행할 경우 공유 변수인 counter에 잘못된 값 입력될 수도
		- 이와 같은 문제는 멀티스레딩, 멀티프로세서가 발달할수록 더 중요한 문제
		- 이를 해결하기 위해 **프로세스 동기화**가 필요

<br>

### Critical Section

![critical section](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter5/5_01_CriticalSection.jpg)

<br>

- **임계구역(critical section)** : 공유 자원을 변경/수정하는 작업을 하는 코드
	- entry section과 exit section
	- remainder section

임계구역 문제 해결 조건
1. **상호배제(mutual exclusion)**
	- 프로세스 P가 임계구역을 실행 중일 때, 다른 프로세스는 자신의 임계구역에 진입할 수 없다.
2. **진행(progress)**
	- 임계구역을 실행 중인 프로세스가 없고, 이를 실행하기 위해 대기 중인 프로세스들이 있을 때, 이들 중 하나를 선택할 수 있고 선택은 무한정 연기될 수 없다.
3. **한정된 대기(bounded waiting)**
	- 임계구역 진입을 요청했을 때부터 그 요청이 허용될때까지 다른 프로세스들이 임계구역에 진입하는 횟수에 한계가 있어야한다.<br>
	=> 무한정 기다리지 않아야 하니까

<br>

## Peterson's Solution

- 두 개의 프로세스 사이 임계구역 문제를 소프트웨어적으로 해결하는 방법

![peterson's solution](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter5/5_02_Petersons.jpg)

👆 P<sub>i</sub>의 코드

<br>

int turn : 현재 어떤 프로세스가 임계구역에 진입할 차례인지를 나타냄<br>
bool flag[2] : 어떤 프로세스가 임계구역에 진입할 준비가 되었는지를 나타냄(true일 때 준비됨 상태)


- 임계구역에 진입하기 위해,
	- flag[현재 프로세스] = true
	- turn = 다른 프로세스
		- 다른 프로세스가 진입할 수 있게
		- turn을 동시에 변경한다해도 결과는 둘 중 하나

<br>

### Peterson's Solution은 임계구역 문제 해결 조건 만족

- 1번 조건 만족
	- flag[0] = flag[1] = true 일 수 있다.
	- turn은 0과 1 둘 중 하나

	- 따라서, 상호 배제(mutual exclusion) 만족 보장
		- 프로세스 P<sub>i</sub>가 임계구역에 진입하려할 때, 아래 조건 만족하면 임계구역 진입
			- flag[j] == false || turn == i
		- flag[j] == true && turn == i 일 때,
			- P<sub>i</sub>가 임계구역을 빠져나온 후 turn = j 를 실행하면 비로소 P<sub>j</sub>는 임계구역 실행 가능해짐

- 2, 3번 조건 만족
	- P<sub>i</sub>가 임계구역에 진입하려할 때,
		- while문 조건 만족하지 않으면 임계구역 진입
		- flag[j] == true && turn == j 인 경우에만 while문 돌며 대기
			- P<sub>j</sub>가 임계구역에 진입할 수 있는 조건 만족(상호 배제)
			- P<sub>j</sub>가 임계구역을 나와 flag[j] = false (P<sub>i</sub>의 while문 조건 만족하지 않게 됨)
				- P<sub>i</sub> 임계구역 진입(while문 빠져나옴)
			- P<sub>j</sub>가 다시 반복문을 돌며 turn = i
				- P<sub>i</sub> 임계구역 진입(while문 빠져나옴)

&nbsp;&nbsp;&nbsp;=> 따라서, P<sub>i</sub>는 임계구역에 진입할 수 있고(2. 진행(progress) 조건 만족) 최대 하나의 프로세스(P<sub>j</sub>)가 임계구역 진입을 기다릴 수 있다.(3. 한정된 대기(bounded waiting) 조건 만족)

<br><br>

reference: A. Silberschatz, P.B. Galvin, and G. Gagne, _Operating System Concepts Essentials_, 2nd ed. Hoboken, NJ, USA: Wiley, 2014.