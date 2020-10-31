---
title : "스파크로 매우 단순한 모델링하기"
excerpt : "Pyspark, RDD, Kitkit School"
category :
  - spark
tag :
  - spark
  - kitkit 
  - educational game
use_math : true
author_profile : true
header:
  teaser : /assets/images/category/data.jpg
  overlay_image : /assets/images/category/data.jpg
  overlay_filter: 0.1
---

데이터를 가지고 간단한 모델링을 수행해보겠습니다. 여기서는, 문제를 풀 때마다, 응답시간(time response)와 현재 풀고 있는 문제의 시도횟수가 어떤 관련이 있는지를 모델링해보겠습니다. 

- 독립변수: 응답시간의 기초 통계
- 종속변수: 문제의 시도 횟수 (정답률과 같은 개념)

## 처리해야하는 로그 데이터의 구조

가공된 데이터는 아래와 같이 단순화되어 있습니다. 데이터는 8명의 사용자가 "Missing Number" 게임을 플레이한 정보를 담고 있습니다. 로그 데이터의 예시는 다음과 같습니다. 

```python
[{'action': 'Start',
  'class': 'Game',
  'device': Row(device_id='5A23001564', full_id='X-57-5A23001564', group_id='X-57', package='com.enuma.xprize', period='Phase2'),
  'info': Row(log=Row(index=Row(global=1523, pack=1000), path='/GAME/sw-TZ_M_02/Free/MissingNumber-002', transition='legal'), session=Row(begin=Row(EAT='2018-05-23 12:13:38.604000+03:00', UTC=1527066818.604), index=78), time=Row(device=1527060098, network=Row(EAT='2018-05-23 13:25:31.626480+03:00', UTC=1527071131.62648, updated=True), time_from_prev=Row(global=0.03650403022766113, pack=0.03650403022766113), time_to_next=Row(global=28.686057090759277, pack=None))),
  'result': Row(connect_at=None, connect_error=None, day=None, expected=None, index=None, is_correct=None, level=None, page=None, position=None, quiz_type=None, response=None, time_delta=None, updated=None),
  'target': 'MissingNumber-002',
  'touch': [],
  '.func': 'game_Begin'}]
```

## 문제 풀이 단위로 로그 데이터를 묶기

우리의 분석 단위는 "문제 풀이"입니다. 문제 풀이는 다음과 같이, solve했거나 quit한 것이 데이터 한줄의 단위가 됩니다. 

- 문제를 시작(start)해서 정답을 맞춰서 (answer하고, result.is_correct==True) Solve함
- 문제를 시작(start)했다가 Quit함

우리가 만들고자하는 데이터 구조는 아래와 같습니다. 

```python
'pid': 각 줄에 대해 붙인 ID 값으로 , 고유합니다.  
'actions': 이 문제를 풀기 위해 사용자가 취한 행동(액션) 리스트
'times': 사용자가 취한 각 행동(액션)에 대해 걸린 시간으로, 이전 행동 시점 대비 현재 행동 시점의 타임스탬프 값의 차이로 계산됩니다.
'user': 사용자 ID 정보로, ['device']['full_id']에 들어있습니다. 
'g_name_level_problem': 문제풀이의 대상이 된 게임의 레벨, 문항번호 정보입니다. ['info']['log']['path']에 들어있습니다
'is_solve': 문제를 해결했는지 아니면 뒤로가기로 퇴장했는지를 나타냅니다. 
```

데이터 구조화는 아래와 같은 순서로 진행합니다.

1. Grouping: 사용자ID별로 로그를 그룹핑
2. FlatMap: 각 사용자에 대해 문제 풀이 단위로 로그 데이터를 묶어서 리턴함

### Grouping

```python
# 문제 단위 데이터
samples_grouped = samples_save_missing.groupBy(lambda x: x['device']['full_id'])
```

### FlatMap

```python
def group_result_by_problem(group):
    
    user, logs = group # (key, list) 순서쌍을 분해합니다.
    
    logs = sorted(logs, key=lambda x: x.info.log['index']['global']) # 목록을 정렬합니다.
    logs_by_problem = [] # user별 prolbem 단위로 묶인 logs

    basket_id = 0 # 바구니에 id를 부여합니다
    basket=[] # 바구니에 problem 하나가 시작해서 끝날 때까지 한 log씩 주어 담듯이, basket 이라는 list를 생성합니다
    history_basket={}    

    total_encounter_state = [] # 이전에 푼 적 있는지 기록하는 list
    solve_state = [] # 이전에 맞춘 적 있는지 기록하는 list
    quit_state = [] # 이전에 포기한 적 있는지 기록하는 list
    
    action_list = [] #액션을 넣는 리스트.
    time_list=[]
    
    for index, log in enumerate(logs):
        
        action_list.append(log['action'])
        time_list.append(log['info']['time']['time_from_prev']['global'])
        
        
        if log['action'] == 'Answer': # Answer하는 경우
            
            basket.append(log) # 우선 log를 바구니에 담습니다

            if log['result']['is_correct'] == False: # 만약 오답을 기입한 경우
                pass # 아무런 행동을 하지 않고 다음 log로 넘어갑니다
            
            else:

                history_basket={'user': log['device']['full_id'], \
                                'g_name_level_problem': log['target'], 
                                'is_solve': 'solve', \
                                }

                # 지금까지의 바구니를 basket_id와 함께 logs_by_problem에 append해준 후
                logs_by_problem.append(\
                                       {'pid':log['device']['full_id']+'_'+str(basket_id),\
                                        'actions':action_list,\
                                        'times':time_list,\
                                        #'plogs':basket, 
                                       'user':history_basket['user'], \
                                        'g_name_level_problem':history_basket['g_name_level_problem'], 
                                        'is_solve':history_basket['is_solve']
                                       })

                # 바구니를 비워줍니다. 이후 부터는 비운 바구니에 새로 log를 담게 됩니다
                basket=[]
                action_list=[]
                time_list=[]

                # 비우고 난 후 새로운 바구니에 새로운 id를 부여합니다
                basket_id += 1

                # history 바구니를 비워줍니다. 이후 부터는 비운 바구니에 새로 log를 담게 됩니다
                history_basket={}

                # 바구니를 비운 후 state 설정
                total_encounter_state.append(log['target'])
                solve_state.append(log['target'])

        elif log['action'] == 'Complete': # Complete하는 경우
            pass
                    
        elif log['action'] == 'Quit': # Quit한 경우, 마지막 오답인 경우라도 묶어줘야하므로 포함합니다
            
            # Quit에서 발생한 log는 별다른 정보가 없으므로 basket에 담지 않습니다
            
            # 대신 직전에 정의한 history_basket을 아래와 같이 'is_solve' 정보를 수정하여 새로 정의합니다

            if '#' not in logs[index-1]['target']: # 만약 Quit을 했는데, 이전 target에 문제번호가 없다면(시작하자마자 quit)
            
                # 첫문제에서 Quit 한 것이므로(Complete 직후 또는 Quit 직후) 아래와 같이 수정합니다
                history_basket={'user':log['device']['full_id'], \
                                     'g_name_level_problem':log['target']+("/#0"), 'is_solve':'quit', \
                                     }
                       
            else: # 만약 Quit을 했는데, 이전 target에 문제번호가 있다면(풀다가 quit)

                # 직전 문제에서 Quit 한 것이므로 아래와 같이 수정합니다
                history_basket = {'user':log['device']['full_id'], \
                                'g_name_level_problem':logs[index-1]['target'], 'is_solve':'quit', \
                                }
            
            # 지금까지의 바구니를 basket_id와 함께 logs_by_problem에 append해준 후
            logs_by_problem.append(\
                                   {'pid':log['device']['full_id']+'_'+str(basket_id),\
                                    'actions':action_list,\
                                    'times':time_list,\
                                   # 'plogs':basket, 
                                    'user':history_basket['user'], \
                                'g_name_level_problem':history_basket['g_name_level_problem'], 
                                    'is_solve':history_basket['is_solve']})
            
            # 바구니를 비워줍니다. 이후 부터는 비운 바구니에 새로 log를 담게 됩니다
            basket=[]
            action_list=[]
            time_list=[]

            # 비우고 난 후 새로운 바구니에 새로운 id를 부여합니다
            basket_id += 1

            # history 바구니를 비워줍니다. 이후 부터는 비운 바구니에 새로 log를 담게 됩니다
            history_basket={}
            
            # 바구니를 비운 후 state 설정
            total_encounter_state.append(log['target'])
            quit_state.append(log['target'])

            
        else:
            pass

    return logs_by_problem
```

위 코드를 flatMap으로 적용

```python
problem_grouped = samples_grouped.flatMap(group_result_by_problem)
```

끝으로, 우리는 중도 포기하지 않고 문제를 푼 것에만 관심을 가질 것이기에, 필터링을 해야합니다. 

```python
problem_filtered=problem_grouped.filter(lambda x:x['is_solve']=='solve')
```

여기까지 결과의 예시는 아래와 같습니다. 

```python
#problem_filtered.take(1)
[{'pid': 'X-57-6116002428_0',
  'actions': ['Start', 'Answer'],
  'times': [0.03669595718383789, 14.44014310836792],
  'user': 'X-57-6116002428',
  'g_name_level_problem': 'MissingNumber-001/#0',
  'is_solve': 'solve'}]
```

## 관심 있는 Feature 뽑기

모델링을 위해 사용할 피쳐를 아래와 같이 디자인할 것입니다. 

```python
'time_first': 첫번째 응답시간
'time_min': 응답시간의 최소값
'time_max': 응답시간의 최대값
'time_mean': 응답시간의 평균
'time_median': 응답시간의 중위값
'time_final': 마지막 응답시간
'attempt_time': 답 입력 횟수
```

함수를 짜면

```python
import numpy as np
def extract_features(row):
    new_row=dict()
    new_row['time_first']=row['times'][0]
    new_row['time_min']=sorted(row['times'])[0]
    new_row['time_max']=sorted(row['times'])[-1]
    new_row['time_min']=np.mean(row['times'])
    new_row['time_median']=np.median(row['times'])
    new_row['time_final']=row['times'][-1]
    new_row['attempt_time']=np.sum([1 for a in row['actions'] if a=='Answer'])
    
    return new_row

problem_features=problem_filtered.filter(lambda x:x!=None and x['times']!=None and x['actions']!=None).\
filter(lambda x:x['times']!=[]).map(extract_features)
```

결과

```python
[{'time_first': 0.03669595718383789,
  'time_min': 7.238419532775879,
  'time_max': 14.44014310836792,
  'time_median': 7.238419532775879,
  'time_final': 14.44014310836792,
  'attempt_time': 1}]
```

## 선형 회귀 모델링하기

RDD를 데이터 프레임으로 바꿉니다. 

```python
problem_collected=problem_features.collect()
import pandas as pd
df=pd.DataFrame(problem_collected)
```

아웃라이어를 검출해서, 상한값이나 하한값으로 대체합니다. 

```python
def replace_outlier(df):
    q1=np.percentile(df, 25)
    q3=np.percentile(df, 75)
    iqr=q3-q1
    bottom=q1-1.5*iqr
    top=q3+1.5*iqr
    for i,v in enumerate(df):
        if v<bottom:
            df[i]=bottom
        elif v>top:
            df[i]=top
    return df

df_replace=df.copy()
for col in df.columns:
    df_replace[col]=replace_outlier(df_replace[col])
```

Target에 해당하는 데이터를 히스토그램으로 시각화해보겠습니다. 

```python

```