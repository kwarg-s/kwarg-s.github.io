---
title : "운영체제 (5) : 프로세스 구조"
excerpt : "stack,heap,bss,data,code"
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
<img src="../assets/img/os/process-archi.png" style="width:50%;">
</center>  
<p></p>

**프로세스 컴파일**: 프로그램이 기계어로 변환되는 것

1. **CODE**: 프로그램 코드 저장

2. **DATA**: 컴파일시 데이터에 변수를 저장

3. **STACK**: 함수의 리턴 주소와 인자를 저장

4. **HEAP**: 코드에서 동적으로 만들어진 데이터를 저장



## Heap이란?

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

1. 정수형 포인터를 선언하였다.
2. ``malloc()`` : 동적으로 메모리를 생성하는 함수입니다. 인자는 사이즈입니다. 즉, int라는 정수 타입의 사이즈이므로, 32bit의 공간만큼을 동적으로 메모리를 할당한 것입니다. 
3. data 값에는 포인터값이 리턴 됩니다. data는 동적 메모리에 접근할 수 있는 변수입니다.  
4. ``heap공간: 32bit 규모의 동적 메모리``가 생성됩니다. 동적 메모리는 아직 데이터가 저장되지 않은 빈 공간 개념(``heap공간: (int*)=malloc(sizeof(int))``)이므로 0이 있습니다. 한편 stack에는 그 메모리 공간의 주소(포인터 변수인 data)가 저장됩니다.   
  
|프로세스구조|데이터|주소|
|---|---|---|
|stack|data=1000h|EFFFh|
|heap|0|1000h|  

6. ``*data=1`` 해당 공간에 1이라는 데이터를 넣습니다.  
7. ``printf``: 1을 출력하고 끝납니다.   

## DATA

<center>
<img src="../assets/img/os/process-archi2.png" style="width:50%;">
</center>  
<p></p>

- BSS: 초기화되지 않은 전역변수
- DATA: 초기값이 있는 전역변수

1. ``int global_data1;``:전역변수 선언만 함 -> 초기화가 되지 않음 -> BSS에 저장됩니다.
2. ``int global_data2=1;``전역변수에 값을 넣음 -> 초기값이 있음 -> DATA에 저장됩니다.
3. ``함수 속 int *data;``: 지역변수를 선언 함 -> stack에 저장됩니다.

## PCB : 컨텍스트 스위칭  

**PCB**
프로세스 상태를 나타내는 구조체로, 운영체제가 가지고 있다 . PCB는 운영체제가 프로세스에 대한 중요한 정보를 저장해 놓는 곳입니다. 
- 각 프로세스가 생성될 떄마다 고유의 PCB가 생성됩니다.
- 프로세스가 종료되면 PCB가 제거됩니다. 


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




























## Reference
- <a href="https://www.fastcampus.co.kr/dev_online_cs/">fast campus 강의</a>