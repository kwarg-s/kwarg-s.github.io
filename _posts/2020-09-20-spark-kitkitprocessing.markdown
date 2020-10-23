---
title : "스파크로 로그 데이터 전처리하기"
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

포스트는 수업시간에서 박사과정 조교분이 제작해주신 강의노트를 재구성해서 작성하였습니다.

## Kitkit school에서 수집한 로그 데이터

킷킷 스쿨 게임을 플레이하면 docs/폴더에 로그 파일이 저장되는데요

사용자가 게임을 플레이하면서 하나의 "ACTION"을 취할 때마다 한줄의 로그 데이터가 기록이 됩니다.

로그 데이터 한줄의 예시는 아래와 같습니다.

### 로그 데이터 예시

```json
#sample_data[0]
{'appName': 'xprize',
 'timeStamp': 1578385736,
 'event': {'!datetime': '2020-01-07T08:28:56Z',
  '.func': 'touchEvent_Begin_End',
  '0.nodeName': 'WordWindowScene',
  '1.data': 'ObEVOcEVOcEVOcEV',
  '_localTimestamp': 1578385736.5877,
  '_userName': 'user0',
  'action': 'StrictLogManager'},
 'sntp': -1,
 'user': 'user0'}
```

위 데이터를 보시면, 'WordWindow'라는 게임에서 

'touchEvent'가 발생하였음을 알 수 있습니다.

데이터 스키마 형태로 정리하면

### 데이터 스키마

```
|- appName: <class 'str'>
|- timeStamp: <class 'int'>
|- event: <class 'dict'>
|  |- !datetime: <class 'str'>
|  |- .func: <class 'str'>
|  |- 0.nodeName: <class 'str'>
|  |- 1.data: <class 'str'>
|  |- _localTimestamp: <class 'float'>
|  |- _userName: <class 'str'>
|  |- action: <class 'str'>
|- sntp: <class 'int'>
|- user: <class 'str'>
```

사용자가 어떤 게임을 했는지, 어떤 행동을 취했는지에 따라 .func의 종류가 달라집니다. 그리고 이 .func의 종류에 따라 0.nodeName, 1.data등 나머지 데이터 구조도 달라지는데요, 다른 로그 데이터의 예시를 보시면 다음과 같습니다.

### 또 다른 로그 데이터 예시

```json
{'appName': 'xprize',
  'timeStamp': 1578385691,
  'event': {'!datetime': '2020-01-07T08:28:11Z',
   '.func': 'game_Peek_Answer',
   '0.gameName': 'workwindow',
   '1.workPath': '/wordwindow/level-17-3/work-1',
   '2.userAnswer': '3',
   '3.correctAnswer': '3',
   '_localTimestamp': 1578385691.005702,
   '_userName': 'user0',
   'action': 'StrictLogManager'},
  'sntp': -1,
  'user': 'user0'}
```

2번째 데이터 로그는 .func이 'touchEvent_Begin_End'였던 1번째 데이터 로그와는 달리 2번째 데이터 로그의 .func은 'game_Peek_Answer'입니다. '0.nodeName', '1.data' 등의 데이터가 등장하는 1번 로그와는 달리, 2번 로그에서는 '0.gameName', '1.workPath', '2.userAnswer', '3.correctAnswer' 등이 있습니다.

### .func에 올 수 있는 모든 정보

```json
#event_func_type
['touchEvent_Begin_End',
 'dailyGameChoice_ChooseDailyGame',
 'game_Begin',
 'game_Peek_Answer',
 'game_End_Complete',
 'eggQuiz_CorrectAnswer',
 'starStat_UpdateStarInKitKitSchool',
 'dailyGameChoice_End_Complete',
 'dailyGameChoice_Begin',
 'video_Begin',
 'video_End_Complete',
 'eggQuiz_WrongAnswer',
 'game_End_Quit',
 'dayChoice_End_Complete',
 'courseChoice_End',
 'curriculumChoice_TouchCoop',
 'courseChoice_Begin',
 'courseChoice_TouchAnimal',
 'dayChoice_Begin',
 'gameTutorialVideo_Begin',
 'gameTutorialVideo_End',
 'dayChoice_End_Quit',
 'dayChoice_OpenFreeChoiceLevelPopup',
 'dayChoice_ChooseFreeChoiceGame',
 'dayChoice_CloseFreeChoiceLevelPopup',
 'curriculumChoice_Begin',
 'touchEvent_Begin_Cancel',
 'touchEvent_Begin_Begin',
 'video_End_Quit',
 'dailyGameChoice_End_Quit',
 'dayChoice_CloseFreeChoiceMenu',
 'courseChoice_ShowNewAnimal',
 'curriculumChoice_End']
```

.func에 올 수 있는 모든 정보를 정리하면 위와 같이 매우 다양합니다.

## 전체 데이터 로그에 적용할 수 있는 템플릿

로그의 .func마다 스키마 구조가 다르기 때문에, 모든 .func에 대해 나타날 수 있는 키값을 새로운 키값으로 구조화시키고, 이 키들은 event 키 하위에 놓겠습니다. 구조는 다음과 같이 변형할 것입니다.

```json

  |- 'appName'
  |- 'timeStamp'
  |- 'sntp'
  |- 'user
  |- 'event':
      |- event key1
      |- event key2
      |- event key3
      |- ...
```

```python
# part1) .func이 존재하는 데이터의 하위 키값을 저장합니다
# '.func' 종류에 따라 event의 key 값을 반환하는 함수
def key_extraction_from_event_func(event_func_type,sample_data):
    for item in sample_data:
        if '.func' in list(item['event'].keys()): #.func키가 있을 경우           
            if item['event']['.func'] == event_func_type: #해당 .func인 경우에
                first_event_func_data = item 
                break
            else:
                pass
        else: # .func키가 없는 경우 (= DailyScene)
            pass 
    return list(item['event'].keys()) #해당 .func인 item의 event key들의 리스트를 뽑음.

# 모든 event key를 저장할 event_keys_list 생성
event_keys_list = []

# key_extraction_from_event_func를 통해 모든 key 값을 도출하여 template 만들기
for event_func_type in event_func_list:
    event_keys_list.extend(key_extraction_from_event_func(event_func_type,sample_data))

# part2) .func이 조재하지 않는 데이터의 하위 키값도 저장합니다.
for item in sample_data:
    if '.func' in list(item['event'].keys()):
        pass
    else:
        event_keys_list.extend(list(item['event'].keys()))
        break

# part3) 저장된 키값의 중복을 제거합니다.
event_keys_list #모든 이벤트의 키값을 저장하는 리스트
event_keys_list=list(set(event_keys_list)) #중복 제거

'''
['0.problemIndex',
 '1.workPath',
 '2.targetCount',
 '1.duration',
 '2.choiceIndex',
 'action',
 '1.newStars',
 '2.param',
 '3.correctAnswer',
 '1.gameLevel',
 '0.oldStars',
 '1.dayID',
 '1.data',
 'label',
 '1.answer',
 '3.answerIndex',
 '0.videoName',
 '0.nodeName',
 '!datetime',
 '1.correctAnswer',
 '0.gameName',
 '2.myAnswer',
 'category',
 '2.userAnswer',
 '3.result',
 '2.answerIndex',
 '0.levelID',
 '_localTimestamp',
 '2.result',
 '.func',
 '2.duration',
 'value',
 '_userName',
 '1.openCount']
'''
```

### 키값을 가지고 빈 템플릿 만들기

```python
#모든 event 산하의 키들에 대해 None을 집어넣은 템플릿을 만든다. 
event_template = {k : None for k in event_keys_list}
event_template['1.data'] = []

'''
{'!datetime': None,
 '.func': None,
 '0.gameName': None,
 '0.levelID': None,
 '0.nodeName': None,
 '0.oldStars': None,
 '0.problemIndex': None,
 '0.videoName': None,
 '1.answer': None,
 '1.correctAnswer': None,
 '1.data': [],
 '1.dayID': None,
 '1.duration': None,
 '1.gameLevel': None,
 '1.newStars': None,
 '1.openCount': None,
 '1.workPath': None,
 '2.answerIndex': None,
 '2.choiceIndex': None,
 '2.duration': None,
 '2.myAnswer': None,
 '2.param': None,
 '2.result': None,
 '2.targetCount': None,
 '2.userAnswer': None,
 '3.answerIndex': None,
 '3.correctAnswer': None,
 '3.result': None,
 '_localTimestamp': None,
 '_userName': None,
 'action': None,
 'category': None,
 'label': None,
 'value': None}
'''
```

### 빈 템플릿에 데이터 로그 정보를 끼어넣어서 새 데이터 완성

```python
def input_data_to_template(raw_data):
    
    user_event_template = event_template.copy()
    user_event_template.update(raw_data['event']) 
		#빈 템플릿에 raw_data의 event 영역을 채워넣습니다.  
    
    return {
            'appName': raw_data['appName'],
            'timeStamp': raw_data['timeStamp'],
            'sntp': raw_data['sntp'],
            'user': raw_data['user'],
            'event': user_event_template
        }

input_data_to_template(sample_data[10])

'''
{'appName': 'xprize',
 'timeStamp': 1578385793,
 'sntp': -1,
 'user': 'user0',
 'event': {'1.newStars': None,
  'action': 'StrictLogManager',
  '!datetime': '2020-01-07T08:29:53Z',
  '1.data': [],
  '3.answerIndex': None,
  '2.choiceIndex': None,
  '_userName': 'user0',
  '2.answerIndex': None,
  '0.problemIndex': None,
  '1.answer': None,
  '1.duration': None,
  '3.result': None,
  '2.duration': None,
  '0.gameName': 'workwindow',
  '0.nodeName': None,
  '1.gameLevel': None,
  '0.videoName': None,
  '0.levelID': None,
  '2.userAnswer': '1',
  '2.myAnswer': None,
  '1.correctAnswer': None,
  '3.correctAnswer': '1',
  '1.dayID': None,
  '1.workPath': '/wordwindow/level-17-3/work-4',
  '1.openCount': None,
  '0.oldStars': None,
  '2.result': None,
  '.func': 'game_Peek_Answer',
  '_localTimestamp': 1578385793.525363,
  '2.targetCount': None,
  '2.param': None}}
'''
```