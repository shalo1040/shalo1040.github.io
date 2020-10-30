---
layout: post
title : "Thread Libraries"
author : "DAEUN"
---

### 스레드 라이브러리

개발자에게 스레드를 생성하고 관리할 수 있도록 하는 API를 제공한다.

&nbsp;&nbsp;&nbsp; - implementation이 아닌, specification for thread behavior

<br>

### 스레드 라이브러리 구현 방법 2가지

1. user 영역에 구현
	- 커널 관여 x
	- 라이브러리 함수 호출 -> 지역 함수 호출

2. kernel 영역에 구현
	- 커널 관여 o
	- 라이브러리 함수 호출 -> 시스템 콜 호출

<br>

### Asynchronous vs Synchronous Threading

- Asynchronous Threading(비동기)
	- 부모와 자식 스레드는 동시에 실행
	- 자식 스레드가 종료했는지 부모 스레드는 알지 못함
		- 스레드는 독립적이어서 데이터를 거의 공유하지 않음

<br>

- Synchronous Threading(동기)
	- 모든 자식 스레드가 종료할 때까지 기다림(fork-join 방법)
	- 자식들 간 동시 수행, 종료하면 부모에 join
		- data sharing
			- 전역 변수(Pthreads, Windows)
			- reference 전달 using getter, setter(Java)

<br>

### 대표적 스레드 라이브러리 : POSIX Pthreads, Windows, Java

| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Pthreads | Windows | Java |
| :---: | :---: | :---: | :---: |
| thread level | user-level 또는 kernel-level | kernel-level | kernel-level<br>(JVM은 host 운영체제 위에서 동작하기 때문에 host system의 스레드 라이브러리로 구현) |
| thread creation | pthread_create() | CreateThread() | start() |
| wait child | pthread_join() | WaitForSingleObject() 또는 WaitForMultipleObjects() | join() |
| 동기/<br>비동기 | 동기 | 동기 | 동기 |
| 비고 | POSIX 표준 | | |