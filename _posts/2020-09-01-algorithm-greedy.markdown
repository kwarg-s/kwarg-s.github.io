---
title : "탐욕 알고리즘"
excerpt : "Greedy"
category :
  - algorithm
tag :
  - algorithm
use_math : true
author_profile : true
header:
  teaser : /assets/images/main.jpg
  overlay_image : /assets/images/main.jpg
  overlay_filter: 0.1
---

# 탐욕알고리즘

## 탐욕 알고리즘

여러 경우 중 하나를 결정해야할 때마다, 매순간 최적이라고 생각되는 경우를 선택하는 방식으로 진행해서, 최종적인 값을 구하는 방식

### 동전 문제

4720원을 지불할때, 1,50,100,500원 동전으로 동전 수 가장 적게하는 방법

```python
coin_list = [500, 100, 50, 1] 

def min_coin_count(value, coin_list):
    total_coin_count = 0
    details = []
    coin_list.sort(reverse=True) #큰 수부터 내림차순으로 정렬
    
    for coin in coin_list: #큰 동전부터
        coin_num = value // coin #필요한 동전 개수
        total_coin_count += coin_num #더함
        value -= coin_num * coin #남은 돈
        details.append([coin, coin_num]) #[사용한 동전 종류, 사용한 동전 개수] 추가
    return total_coin_count, details #사용한 총 동전 개수, 구체적인 내용

min_coin_count(4720, coin_list)
```

### Fractional Knapsack Problem

배낭에 담을 수 있는 무게의 최댓값이 정해져 있고, 일정 가치와 무게가 있는 짐들을 배낭에 넣을 때 가치의 합이 최대가 되도록 짐을 고르는 방법을 찾는 문제. 분할가능 배낭문제는 짐을 쪼갤 수 있는 경우에 해당한다.

순간순간의 선택이 이후 선택에 영향을 주지 않고, 순간순간의 최적 선택이 전체 문제 최적해와 일치한다. 따라서 분할가능 배낭문제는 탐욕 알고리즘으로 풀 수가 있다.

```python
def get_max_value(data_list, capacity):
    
    #가치/무게를 기준으로 내림차순으로 정렬: 무게 대비 가치가 높은 짐부터 실어야한다. 
    data_list = sorted(data_list, key=lambda x: x[1] / x[0], reverse=True)
    
    
    total_value = 0
    
    #디테일
    details = list()
    
    for data in data_list:#각 짐에 대해
        
        # 용량이 짐의 무게보다 크면: 그냥 전체를 실으면 된다
        if capacity - data[0] >= 0:
            capacity -= data[0] #전체를 실어서 남는 용량
            total_value += data[1] #가치를 추가
            details.append([data[0], data[1], 1]) #디테일
        
        # 용량이 짐의 무게보다 모자라는 경우 : 남은 짐을 배낭이 꽉찰 정도로만 쪼개서 넣은 다음 break한다.
        else:
            fraction = capacity / data[0] #짐의 무게 대비 용량
            total_value += data[1] * fraction #
            details.append([data[0], data[1], fraction])
            break
    return total_value, details

data_list = [(10, 10), (15, 12), (20, 10), (25, 8), (30, 5)]
get_max_value(data_list, 30)
#(24.5, [[10, 10, 1], [15, 12, 1], [20, 10, 0.25]])
```