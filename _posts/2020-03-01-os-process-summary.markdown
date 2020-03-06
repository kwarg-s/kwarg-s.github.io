---
title : "운영체제 (5) : [정리] 프로세스가 실행되는 방법"
excerpt : "stack,heap,bss,data,code,Process control block"
category :
  - os
tag :
  - os
use_math : true
author_profile : true
header:
  teaser : /assets/images/category/os.jpg
  overlay_image : /assets/images/category/os.jpg
  overlay_filter: 0.1
---

## 프로세스 개념 정리

- 스케쥴러 : 어느 순서대로 프로세스를 실행시킬 것인지를 결정합니다.
  - 비선점형: FIFO, SJF, Priority-based
  - 선점형: 시분할, RobinRound
- 프로세스 상태 : new-ready-running-block
- 인터럽트 : cpu가 프로그램을 실행하고 있을 때 입출력 장치나 예외상황이 발생해서 프로세스의 상태를 바꾸는 것입니다.
  - 입출력이 발생 : 발생 사실을 os에 알려줍니다.
  - 입출력이 끝남 : block에서 ready로 되돌립니다. 
  - 예외상황 발생 : running 중에 예외가 발생하여 중단합니다.
  - 타이머 인터럽트: 선점형 스케줄러를 구현하기 위해 하드웨어는 일정한 시간 구간마다 인터럽트를 운영체제 측에 알려줍니다.  
- IDT: 시스템콜 번호를 입력하면 커널 모드의 함수 주소 위치를 알려주는 표입니다. 예를 들어 0x80을 입력하면 system_call() 커널 함수 코드를 실행합니다.
- 프로세스의 구조 (프로세스의 메모리 구조) : 프로세스에게는 4GB의 가상 메모리 공간이 할당됩니다.  
  - stack: 함수의 리턴 주소, 인자, 지역변수가 저장됩니다. 
  - heap: 코드에서 동적으로 만들어진 데이터를 저장합니다.
  - bss: 초기화가 되지 않은 전역 데이터 변수가 저장됩니다. 
  - data: 초기화가 된 전역 데이터 변수가 저장됩니다.
  - code: 프로그램 코드 저장.
- PCB: 프로세스 상태를 저장하기 위해 운영체제가 저장해 놓는 곳이며, 메모리에 저장됩니다. 
  - process id
  - 레지스터 값 : PC, SP
  - 프로세스 상태
  - memory info 등
- 컨텍스트 스위칭: cpu에 실행할 프로세스를 교체하는 기술입니다. 








## 프로그램이 실행되는 방법

<center>
<img src="../assets/img/os/process-all.png" style="width:80%;">
</center>  
<p></p>

- 프로그램: .c 파일이 컴파일되면 실행파일로 변환됩니다.
- 사용자: 한편 사용자는 shell(cli,gui)을 통해 운영체제 측에 프로그램 실행을 요청합니다.
- 실행파일의 구조(프로세스의 구성)가 만들어집니다: stack, heap, bss, data, text 로 구성됩니다. 
- 이 파일은 ready상태입니다. 
- 인터럽트: 하드웨어는 운영체제에게 일정 시간 간격(0.01초)으로 타이머 인터럽트를 보냅니다. 
  - cpu는 사용자모드를 커널모드로 바꿉니다.
  - 0x80 (시스템콜 인터럽트)에 해당하는 커널 함수를 주소값을 확인해서, 커널 함수를 실행합니다. 
  - 만약 컨텍스트 스위칭 함수 알고리즘를을 ``인터럽트를 5번 받았을 때``라고 정했다면, 0.05초가 되었을때 
  - running을 ready로 바꿔주는 컨텍스트 스위칭을 합니다. 
- 이전 프로세스가 ready되었으므로 새 프로세스가 running 상태가 될 수 있습니다.
- cpu는 프로세스 구조에 있는 code를 실행하기 시작합니다. 
- c코드의 경우, 헤더 파일을 읽는순간 헤더 파일에 구현된 라이브러리(=api)를 가져오고 여기에는 open() 등의 함수가 있습니다. 



open()함수 코드는 다음과 같다라고 가정할게요. 
```
move eax, 5 
move ebx, 0
int 0x80 // cpu가 제공하는 opcode. 인터럽트 명령을 내리는 함수 번호
```
여기서  
  - 0x80은 실행(되는) 커널 함수 주소
  - eax값 1은 시스템콜(sys_call) 처리 커널 함수 주소  

    idt   

    |opcode|커널 함수가 있는 주소|
    |-----|----|
    |0x00|divide_error() 커널 함수가 있는 주소|
    |0x80|system_call()이라는 커널 함수가 있는 주소|

- 커널 함수 실행 : 커널에서 system_call() 함수가 있는 주소를 찾아가서 system_call() 코드를 실행합니다. 그런데 이 코드에서는 sys_call_table(인수=eax)를 호출합니다.  

  sys_call_table  

  |%eax|sys_call 커널 함수|%ebx|
  |---|---|---|
  |1|sys_exit(exit)|int|
  |3|sys_read(read)|unsigned int|
  |5|sys_open(open)|const char*|



- eax=5에 해당하는 sys_open()을 실행함에 따라 프로세스는 running에서 block상태로 전환됩니다. 

- 저장매체로부터 데이터를 다 받아오면, 또다시 0x80에 해당하는 system_call()이 실행되고 그결과 block 상태가 ready상태로 돌아옵니다. 

- 타이머 인터럽트 5번 실행되면 ready상태가 running상태로 넘어갑니다. 그럼 아까 끝난 바로 직후부터 코드가 실행됩니다. 

  - 빈번한 IO는 막대한 시간이 듭니다. 
  - spark: 모든 데이터를 메모리에 한꺼번에 올려서 처리하려고 노력합니다. 



## Reference
- <a href="https://www.fastcampus.co.kr/dev_online_cs/">fast campus 강의</a>
- https://recorda.tistory.com/entry/20160503%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B5%AC%EC%A1%B0 