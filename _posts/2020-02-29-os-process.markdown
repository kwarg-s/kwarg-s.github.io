---
title : "운영체제 (5) : 프로세스 구조, PCB, 컨텍스트 스위칭"
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

## **프로세스의 구조** 

<center>
<img src="../assets/img/os/process-archi2.png" style="width:50%;">
</center>  
<p></p>

**프로세스 컴파일**: 프로그램이 기계어로 변환되는 것

1. **CODE**: 프로그램 코드 저장
2. **DATA**: 컴파일시 데이터에 변수를 저장
3. **HEAP**: 코드에서 동적으로 만들어진 데이터를 저장
4. **STACK**: 함수의 리턴 주소와 인자를 저장

<center>
<img src="../assets/img/os/process-archi3.png" style="width:70%;">
</center>  
<p></p>

- 시작 상태 :
  - data : c=0
  - SP,EBP : 1000h
- PC=0000h,0001h,0002h : 아무 실행이 안됨.
- PC=0003h :
  - 함수가 실행됩니다.
  - stack에 EBP값인 1000h가 기록됩니다.
  - 함수값의 return address ret=0004h가 기록됩니다.
  - 두 인수값 a=1, b=2가 기록됩니다.
  - SP와 EBP값은 현재의 새로운 stack 주소인 0FFCh로 기록됩니다.
  - 함수를 실행시키면서 stack의 데이터가 끝에서부터 지워집니다.
  - 결과값은 eax에 저장됩니다(3).
- PC=0004h :
  - 함수의 결과값이 변수 c에 저장되면서
  - 그 값이 Data 영역에 에 기록됩니다 (c=3)
- PC=0005h : 3이 출력됩니다.

--- 
### 프로세스 구조: Heap

동적으로 메모리를 할당하는 것

```c
#include <stdio.h>
#include <stdlib.h>
int main(){
  int *data;
  data = (int *) malloc(sizeof(int));
  *data=1;
  printf("%d\n",*data);
  return 0;
}
```

1. ``int *data`` : 정수형 포인터를 선언하였습니다.
2. ``malloc()`` : 동적으로 메모리를 생성하는 함수입니다. 인자는 사이즈입니다. 즉, int라는 정수 타입의 사이즈이므로, 32bit의 공간만큼을 동적으로 메모리를 할당한 것입니다. 
3. ``data= (int *)``: data 값에는 포인터값이 리턴 됩니다. data는 동적 메모리에 접근할 수 있는 변수입니다.  
4. ``heap공간: 32bit 규모의 동적 메모리``가 생성됩니다. 동적 메모리는 아직 데이터가 저장되지 않은 빈 공간 개념(``heap공간: (int*)=malloc(sizeof(int))``)이므로 0이 있습니다. 한편 stack에는 그 메모리 공간의 주소(포인터 변수인 data)가 저장됩니다.   
  
|프로세스구조|데이터|주소|
|---|---|---|
|stack|data=1000h|EFFFh|
|heap|0|1000h|  
  
6. ``*data=1`` 해당 공간에 1이라는 데이터를 넣습니다.  
7. ``printf``: 1을 출력하고 끝납니다.   

--- 
### 프로세스 구조: BSS , DATA

- BSS: 초기화되지 않은 전역변수
- DATA: 초기값이 있는 전역변수

1. ``int global_data1;``:전역변수 선언만 함 -> 초기화가 되지 않음 -> BSS에 저장됩니다.
2. ``int global_data2=1;``전역변수에 값을 넣음 -> 초기값이 있음 -> DATA에 저장됩니다.
3. ``함수 속 int *data;``: 지역변수를 선언 함 -> stack에 저장됩니다.

### 프로세스 간 공유메모리  

<center>
<img src="../assets/img/os/processmemory.png" style="width:70%;">
</center>  

공유 메모리는 여러 개의 프로세스에서 공통으로 사용할 수 있는 메모리 영역이며, 시스템 콜에 의해 작성됩니다. attach를 통해 프로세스는 공유메모리에 접속합니다.  

## PCB

### **PCB (Process Control Block)의 개념**
프로세스 상태를 나타내는 구조체로, 운영체제가 가지고 있습니다 . PCB는 운영체제가 프로세스에 대한 중요한 정보를 저장해 놓는 곳입니다. 
- 각 프로세스가 생성될 떄마다 고유의 PCB가 생성됩니다.
- 프로세스가 종료되면 PCB가 제거됩니다. 

### **PCB의 구성**
프로세스가 실행 중인 상태를 캡쳐(구조화)해서 저장해줍니다.  
<center>
<img src="../assets/img/os/pcb.png" style="width:50%;">
</center>  
<p></p>

**핵심 구성요소**  
1. Process ID
2. Register 값 : **PC, SP** 등
3. Scheduling Info: 프로세스 상태(ready/block/running)
4. Memory Info: 메모리 사이즈 limit
**부연설명**
- PCB는 c structure로 구성됩니다.
- Processors는 cpu 정보입니다.   
- Priority: 우선순위 정보를 스케줄링에 적용할 수 있습니다. 
- 프로세스마다 부모-자식 구조가 있습니다. 


### **PCB와 컨텍스트 스위칭**
프로세스 B를 running하다가 프로세스 A로 스위칭하는 경우
1. 현재 프로세스 B를 running하면서의 CPU 레지스터 값을 프로세스 B의 PCB에 저장합니다.
  - pC
  - SP
1. 일단 프로세스 A의 PCB를 확인합니다. 
  - PC(프로그램 카운터)
  - SP(스택 포인터)
2. CPU의 레지스터에 PCB 값을 덮어 씌웁니다.
  - PC
  - SP
3. 실행합니다.


## 컨텍스트 스위칭

**컨텍스트 스위칭 (문맥 교환)**
**CPU에 실행할 프로세스를 교체하는 기술** 입니다. 컨텍스트 스위칭도 일종의 실행코드입니다. 
1. 중지할 프로세스A 정보는 프로세스A의 PCB에 업데이트하며, 메인메모리에 저장합니다.
2. 실행할 프로세스B 정보는 메인 메모리에 저장된 PCB 정보를 CPU의 레지스터에 불러옵니다. 
- 디스패치: ready 상태를 running 상태로 바꾸고 실행시키는것.
- 컨텍스트 스위칭은 1/100초 간격으로 컨텍스트 스위칭이 발생합니다. 
- 컨텍스트 스위칭은 어셈블리어로 작성될 때가 많습니다. -> 속도를 최대한 빠르게 합니다. 왜냐하면 일반적인 언어는 컴파일러 프로그램이 필요하고, 컴파일을 하면 시간이 느려지기 때문입니다. 요즘엔 거의 비슷하긴 하답니다. 
- 리눅스의 경우, 다양한 cpu를 지원하는데 각 cpu별로 컨텍스트 스위칭 코드가 별도로 있습니다.  


## IPC 기법

**IPC (Inter Process Communication)**

CPU안 코어는 여러개라서, 각 프로세스는 각 코어에서 동시에 실행시킬 수 있습니다. (병렬처리)

예: 1+2+....+10000
- fork()로 10개 프로세스로 나누어서
- 1~1000, 1001~2000... 각각의 프로세스를 동시에 실행시킵니다.
- 10개의 더한 값을 수집해야하기 때문에 프로세스간 통신이 필요합니다. 

예: 웹서버
- 웹서버는 클라이언트가 요청을 하면 html 파일을 클라이언트에 제공해야 합니다. 
- 새로운 클라이언트의 요청이 있을 때마다, 웹서버는 새로운 프로세스를 만들어서(fork()) 대응해야합니다.
- 이때, cpu 병렬 처리가 가능하면 빨라집니다.
- 각 프로세서는 상태 정보를 교환을 해야하므로 통신이 필요합니다.

1. shared.txt(file) :프로세스 끼리는 저장매체를 공유합니다. 저장매체 내에 결과값을 저장해서 공유하는 방법이 있습니다. 하지만, 실시간으로 데이터가 변하는것을 다른 프로세스가 알게하기 힘듭니다. 디스크를 쓰고 읽는 것을 반복해야하기 때문에 block state, interrupt 등을 거쳐야해서 그 과정이 매우 복잡해집니다. 실제 프로세스 공간은 더 복잡합니다. 사용자 모드의 코드는 kernel space에 접근할 수 없습니다.  
2. pipe: 단방향 통신 (부모->자식)
3. message queue: 프로세스 간 양방향 통신
4. shared memory: 커널 공간에 메모리를 만든다.
4. 시그널: 커널,프로세스에서 다른 프로세스에 발생한 이벤트를 알려주는 기법  
5. 소켓: 소켓은 네트워크 통신을 위한 기술. 




## Reference
- <a href="https://www.fastcampus.co.kr/dev_online_cs/">fast campus 강의</a>