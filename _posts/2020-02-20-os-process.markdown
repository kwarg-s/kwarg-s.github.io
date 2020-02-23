---
title : "운영체제(4) : 스케줄링 알고리즘"
category :
  - subinium
tag :
  - me
  - diary
  - subinium
use_math : true
author_profile : true
header:
  teaser : /assets/images/category/subin.jpg
  overlay_image : /assets/images/category/subin.jpg
  overlay_filter: 0.1
---



#### **프로세스** 

**프로세스**: 실행 중인 프로그램
프로세스는 메모리에 올려진다음
모든 코드는 메모리에 올려진다음 한줄씩 cpu에 넣어져서 실행됩니다.  

코드 이미지(바이너리)는 일종의 실행 파일이며, 리눅스 계열에서는 ELF 포맷으로 되어있습니다.   

**프로세스 = 작업 = task = job**  

작은 차이가 있긴하지만 유사한 용어로 혼용이 됩니다.

응용프로그램은 프로세스와는 다른 개념입니다. 응용 프로그램은 여러개의 프로세스로 이루어질 수 있습니다. 특히 유닉스 계열에서는 하나의 응용 프로그램이 사용자가 원하는 기능을 위해서 여러개의 프로세스가 서로 상호작용하며 실행되기도 합니다. 간단한 프로그램의 경우 하나의 프로세스로 구성될 수도 있습니다.  

#### **스케쥴링 알고리즘**

**스케쥴러**: 누가 프로세스 실행을 관리할 것인지  

**스케쥴링 알고리즘**: 어느 순서대로 프로세스를 실행시킬 것인지
    -시분할 시스템: 프로세스 응답 시간 최소화
    - 멀티 프로그래밍: cpu 활용도를 최대화
    - FIFO 스케쥴러  

#### **스케쥴링 알고리즘: FIFO 스케쥴러**
<center>
<img src="../assets/img/os/fifo1.png" style="width:90%;">
<img src="../assets/img/os/fifo2.png" style="width:90%;">
</center>

프로세스가 저장매체를 읽거나, 프린팅을 하는 등 다른 작업을 하지 않고 처음부터 끝까지 cpu만 사용하는 프로그램이라고 가정합니다.

가장 간단한 스케쥴러입니다. 배치 처리 시스템과 유사합니다. FCFS(First Come First Served) 스케쥴러라고도 합니다. 

#### **스케쥴링 알고리즘 : SJF Short Job First 스케쥴러**

<center>
<img src="../assets/img/os/sjf.png" style="width:90%;">
</center>

SJF Shortest Job First 최단 작업 우선 알고리즘으로, 실행시간이 짧은 프로세스부터 순서대로 실행을 시킵니다. FIFO에 비해 응답시간이 더 짧을 수 있습니다. 



RTOS RealTime OS : 응용 프로그램 실시간 성능 보장을 목표로 하는 OS
- 정확하게 프로그램 시작, 완료 시간을 보장해야 합니다. 
- 시간에 민감한 프로그램을 돌려야하는 환경.
- 하드웨어 RTOS, 소프트웨어 RTOS

GPOS General Purpose OS : 프로세스 실행 시간에 민감하지 않고 일반적인 목적으로 사용되는 OS
- 윈도우, 리눅스

#### **스케쥴링 알고리즘 : Priority-Based 스케쥴러**

<center>
<img src="../assets/img/os/priority.png" style="width:90%;">
</center>

Priority-Based 우선순위 기반 스케쥴러
프로세스에 미리 우선순위를 매겨놓고, 그 순서로 프로세스를 실행시킵니다.우선순위를 매기는 기준은 정하기 나름입니다.
- 정적 우선 순위: 프로세스마다 우선순위를 미리 지정합니다.
    - 예) 카카오톡 관련 프로세스는 우선순위를 낮게, 
- 동적 우선 순위: 스케쥴러가 상황에 따라 우선순위를 동적으로 변경해줍니다. 
    - 예) 실행한지 매우 오래된 응용 프로그랢이 있으면 우선순위를 동적으로 높게 매깁니다. 

#### **스케쥴링 알고리즘 : Round Robin 스케쥴러**

<center>
<img src="../assets/img/os/robinround1.png" style="width:90%;">
</center>

시분할 시스템을 기본으로 합니다. 프로세스를 잘게 쪼개서, 적절히 다른 프로세스로 전환하면서 프로그램을 처리합니다. 

<center>
<img src="../assets/img/os/robinround2.png" style="width:90%;">
</center>

#### **정리**

|스케쥴링 알고리즘|설명|
|--|--|
|FIFO|배치 처리 시스템과 유사|
|SJF|Short Job First|
|Priority-Based|정적 우선순위/동적 우선순위|
|RoundRobin|시분할 시스템 기반|


#### **프로세스 상태**
<center>
<img src="../assets/img/os/processstate.png" style="width:70%;">
</center>

- **``ready (실행 가능)``** : cpu에서 실행 가능 대기 상태
- **``running (실행 중)``** : 현재 cpu에서 실행 중. 만약 싱글 코어 프로세서라면, running인 프로세스는 최대 1개, 최소 0개 입니다.
- **``block (대기)``** : wait상태와 같음. 특정 이벤트 발생 대기 상태. 만약 저장 매체에 파일 읽기를 요청했다고 했을 때, 프로세스는 파일 읽기를 할때까지 기다리는데 이것이 block상태입니다. 파일을 다 읽으면 ready상태로 돌아갑니다.
- **``exit (종료)``**: 시스템 리소스를 풀어주려고 하는 상태.

#### **프로세스 상태 간 관계**
<center>
<img src="../assets/img/os/processstate2.png" style="width:50%;">
</center>

- ``running 상태``에서부터 시작
- (1) 파일 읽기를 요청하면 ``block 상태``로 변합니다.
- (2) 파일 읽기가 끝나면 ``ready 상태``로 갑니다. 스케쥴러는 다음에 실행할 프로세스가 뭐가 있는지 확인할 때 ready상태인 프로세스가 무엇인지 봅니다.
- (3) 프로세스가 ``running 상태``로 바뀝니다. 
- (4) 시분할 시스템에서는 주기적으로 다른 프로세스로 바꿔주는데, 이때 running하던 프로세스를 ``ready 상태``로 바꿔줍니다. 




#### 

running 상태: -
ready 상태: 1,2,3
block 상태: -

running 상태: 2
ready 상태: 1,3
block 상태: -

running 상태:
ready 상태: 1,3
block 상태: 2

ready 상태인 프로세스가 여러개일 때, 어떤 프로세스부터 run해야할까? 그에 대한 계획이 필요합니다. 

- Ready State Queue
- Running State Queue
- Block State Queue









### Reference
- <a href="http://www.fastcampus.co.kr">fast campus 강의</a>