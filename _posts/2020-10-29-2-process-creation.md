---
layout : post
title : "Process Creation"
author : "DAEUN"
---

<img src="https://media.cheggcdn.com/media%2Fb32%2Fb322e832-85cb-4907-ade0-d0c8abbb26c8%2FphpDveJTB.png" width="700">

<br>

- 프로세스는 실행되는 동안 여러 프로세스를 생성할 수 있다.
- 부모 프로세스와 자식 프로세스 간 **트리** 형성
- 각 프로세스는 process identifier(pid)를 가진다.
	- integer value
	- 프로세스 관리 및 접근을 위한 식별자

```
- 자식 프로세스 생성 시,
	- 자원
		1. 운영체제로부터 자원 할당
		2. 부모의 자원 사용
			- 모든 자원 공유
			- 자원의 일부 공유
	- 실행
		1. 부모와 자식이 병행하게(concurrently) 실행
		2. 자식 프로세스가 종료될 때까지 부모는 기다림
	- 주소
		1. 자식은 부모의 주소 공간을 복제
			- 자식과 부모 간 통신 용이
		2. 자식은 부모의 공간 안에 새로운 프로그램을 적재
```

<br>

### UNIX에서의 프로세스 생성

![process creation in UNIX system](https://i.stack.imgur.com/22pF4.png)

<br>

- fork() : 자식 프로세스 생성하는 시스템 콜
	- 자식 프로세스 생성 실패했을 때, 음수 값 리턴
	- 자식 프로세스일 때, 0 리턴
	- 부모 프로세스일 때, 양수 값 리턴(이 값은 자식 프로세스의 pid)
	- 부모 및 자식 프로세스는 코드의 fork() 다음 명령부터 병행 실행
- 실행 : (1)부모와 자식은 병행하게 실행
- 주소 : (1)자식은 부모의 주소 공간을 복제
- exec() : 프로세스의 메모리 공간에 새로운 프로그램으로 대체하는 시스템 콜
	- 부모 또는 자식 프로세스 중 하나가 이를 실행
- wait() : 호출한 프로세스의 상태를 ready로 변경
	- 자식 프로세스가 실행되는 동안 동작을 할 필요가 없다면 이를 실행해 자식이 끝날 때까지 기다릴 수 있음

<br>

### Windows에서의 프로세스 생성

![process creation in Windows system](/assets/images/windows_code.PNG)

<br>

- CreateProcess() : UNIX의 fork() + exec()
	- 10개의 매개변수 전달
	- 자식의 주소 공간에 특정 프로그램을 적재
- 실행 : (1)부모와 자식은 병행하게 실행
- 주소 : (1)자식은 부모의 주소 공간을 복제
- WaitForSingleObject() : UNIX의 wait()

<br><br>

reference: A. Silberschatz, P.B. Galvin, and G. Gagne, _Operating System Concepts Essentials_, 2nd ed. Hoboken, NJ, USA: Wiley, 2014.