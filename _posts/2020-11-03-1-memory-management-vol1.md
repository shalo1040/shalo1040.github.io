---
layout : post
title: "메모리 관리(1)"
author: "DAEUN"
---

- 프로그램이 실행되기 위해서는 메모리에 적재 되어야 하고
- CPU가 직접 접근할 수 있는 저장장치는 메인 메모리와 레지스터가 유일
	- 레지스터는 1 CPU clock이면 접근 가능하지만,
	- 메인 메모리는 더 많은 사이클을 필요로하고
	- **멈춤현상(stall)** 발생할 수 있다.

=> **캐시(cache)**로 메모리 접근 더 빠르게 함(자주 쓰는 data 적재)
=> 속도뿐만 아니라 올바른 연산을 하기 위해 메모리를 보호하는 것이 필요

<br>

### 기준(Base) 및 상한(Limit) 레지스터


![memory](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter8/8_01_LogicalAddressSpace.jpg)

<br>

- base register : 할당된 물리적 메모리의 시작 주소
- limit register : 주소 공간에서 차지하는 크기 정의
- 사용자는 base register가 가리키는 주소부터 (base+limit)이 가리키는 주소까지 안전하게 접근할 수 있다는 의미

<br>

![base,limit](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter8/8_02_HardwareAddressProtection.jpg)

<br>

- 접근할 수 있는 주소 범위가 아닌 경우 trap(software interrupt) 에러 발생
- 사용자 모드에서 생성되는 모든 메모리 주소가 그 사용자의 기준과 상한 범위에 속하는지 검사
- 오로지 OS만이 base/limit 레지스터에 접근 및 변경할 수 있다.
- 운영체제는 전체 메모리 영역에 접근 가능

<br>

### 주소 바인딩(Address Binding)

![binding](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter8/8_03_MultistepProcessing.jpg)

<br>

- 사용자 프로그램은 실행되기까지 여러 단계를 거치며, 각 단계마다 주소는 다르게 표현된다.
- 서로 다르게 표현된 주소를 맵핑
	- 소스 코드 : symbol
		- 변수 "count"
	- 컴파일러가 소스코드를 재배치 가능한 주소로 bind
		- 예. "이 모듈의 시작점으로부터 14바이트"
	- 링커 또는 로더가 재배치 가능한 주소를 절대 주소로 bind

<br>

- 논리 주소
	- CPU에 의해 생성
	- 가상 주소(virtual address)라고도 불림
- 물리 주소
	- 메모리 장치에서 인식되는 주소
- 논리 주소 공간
	- 프로그램에 의해 생성된 논리 주소 집합
- 물리 주소 공간
	- 논리 주소에 상응하는 물리 주소 집합

<br>

- 논리 주소를 물리적 주소로 바인딩하는 3개의 시점
	- **compile time**
		- 컴파일 시점에 주소 결정
		- 메모리의 시작 위치를 알고 있다면 **절대주소(absolute code)** 생성
		- 즉, 프로그램 내부에서 사용하는 논리 주소와 저장될 물리적 주소가 같다.
		- 예. int data = 10  =>  STORE #198000, 10  =>  물리 주소도 198000
		- absolute code : 어셈블리가 정한 위치에 물리적으로 고정된 코드
		- 시작 주소가 변경되면 다시 컴파일
		- 메모리가 비어 있는 경우에 사용 가능
			- 예. 아두이노, 임베디드 시스템
		- 오늘날의 컴퓨터는 이 방법을 사용하지 않음(실행 중 메모리 공간에 다른 프로그램이 차지하고 있을 수 있음)
	- **load time**
		- 프로그램이 메모리에 load될 때 주소 결정
		- 메모리의 시작 주소를 미리 알 수 없다면 컴파일러가 **재배치 가능한 코드(relocatable code)** 생성
		- 예. int data = 10  =>  STORE (.BS+#98000), 10  =>  (시작주소(BS) + 98000)인 물리주로소 맵핑
		- 장점
			- 멀티프로그래밍 가능(컴파일 타임 바인딩은 불가)
		- 단점
			- 변수 및 참조 데이터가 많다면 메모리 맵핑 계산을 여러 번 해야해 메모리 로딩 시 시간이 오래 걸릴 수 있어 비효율적
	- **execution time**
		- 프로그램 실행 시 주소 결정
		- 예. int data = 10  =>  STORE (.BS+#98000), 10  =>  (시작주소(BS) + 98000)
		- load time과의 차이점
			- load time: 메모리 로딩 시 맵핑 작업 => 시간 오래 걸림
			- execution time: 프로그램의 코드가 실행될때마다 반복적으로 주소 맵핑(CPU의 MMU가 이 작업을하며 하드웨어 성능이 발전함에 따라 load time시 맵핑하는 것보다 더 효율적)
		- 프로그램이 실행 중에 메모리 위치를 변경할 수 있다면 이 때 바인딩

<br>

### 메모리 관리기(MMU)

![mmu](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter8/8_04_DynamicRelocation.jpg)

<br>

- **재배치 레지스터(relocation register)** : 기준 레지스터
- 사용자 프로그램은 논리 주소만을 알며, 실제 물리 주소는 알지 못함

<br>

### 동적 적재(Dynamic Loading)

- 필요한 경우에만 메모리 적재
- 아주 간혹 발생하면서 많은 양의 코드를 가진 경우에 유용
- 예. ddl 파일(동적 라이브러리)
- 프로그램 개발자의 몫(not 운영체제)
- 동적 적재하는 이유
	- 프로그램이 실행되기 위해서는 메모리에 적재되어야 하는데
	- 프로세스의 크기는 곧 물리 메모리의 크기에 한정된다.
	- 동적 적재를 통해 메모리 공간을 보다 효율적으로 이용할 수 있다.

<br>

### 동적 링킹(Dynamic Linking)

- 정적 링킹(static linking)
	- 로더가 시스템 라이브러리와 프로그램 코드를 결합해 이진 프로그램 이미지 생성
- 동적 링킹(dynamic linking)
	- 실행 시간까지 링킹 연기
- 스텁(stub)
	- 작은 코드
	- 메모리에 있는 라이브러리 루틴의 위치를 찾음
- 동적 적재와는 달리 운영체제의 도움 필요
	- 다른 프로세스의 메모리에 필요한 라이브러리가 있는지 찾을 필요 있기 때문

<br><br>

refereces : A. Silberschatz, P.B. Galvin, and G. Gagne, _Operating System Concepts Essentials_, 2nd ed. Hoboken, NJ, USA: Wiley, 2014.

[컴파일 시점 바인딩](https://jhnyang.tistory.com/133), [로드, 실행 시점 바인딩](https://jhnyang.tistory.com/246)