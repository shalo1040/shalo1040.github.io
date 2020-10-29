---
layout : post
title : "Process Termination"
author : "DAEUN"
---

### 프로세스 종료 방법

- exit() 시스템 콜
	- 운영체제에게 프로세스 삭제 요청 -> 프로세스의 자원이 운영체제에게 반환
	- 부모에게 상태 값 반환 가능
		- 부모가 wait()을 통해 자식 프로세스가 종료되기를 기다리고 있었다면, wait() 시스템 콜을 통해 종료된 자식 프로세스의 pid와 자식의 상태정보를 반환한다.
- abort()
	- 부모에 의한 강제 종료
		- 자식이 자신에게 할당된 자원을 초과 사용했을 때<br>
			-> 자식의 상태를 체크하는 기능 필요
		- 자식의 task가 더 이상 필요하지 않을 때
		- 부모가 종료될 때<br>
			= 연쇄식 종료(cascading termintation)


1. 직접 종료(강제 종료)
	- exit() 직접 입력
	- abort()

2. 간접 종료
	- return(->exit() 시스템 콜 실행)
	- 안전한 방법

<br>

### 좀비(zombie) 프로세스

자식 프로세스가 종료 되었지만, 부모 프로세스가 wait()을 호출하지 않은 프로세스
	- 모든 프로세스는 일이 끝나면 아주 잠깐 좀비 프로세스 상태임
	- 프로세스가 종료되면 운영체제에 자원을 돌려주지만, process table에는 여전히 프로세스의 상태 정보가 남아 있음
	- 부모가 wait()을 호출했을 때, 자식은 자신의 pid와 상태 정보를 전달하고 완전히 소멸
	- 부모가 wait()을 호출하지 않고 종료되면, 자식은 고아 프로세스가 됨

<br>

### 고아(orphan) 프로세스

wait()을 호출하지 않고 종료된 부모 프로세스의 자식 프로세스
	- LINUX, UNIX의 경우 init 프로세스(root)를 새로운 부모 프로세스로 설정해 해당 자식 프로세스 소멸
		- 주기적으로 init 프로세스가 wait()을 호출한다.

<br><br>

reference: A. Silberschatz, P.B. Galvin, and G. Gagne, _Operating System Concepts Essentials_, 2nd ed. Hoboken, NJ, USA: Wiley, 2014.