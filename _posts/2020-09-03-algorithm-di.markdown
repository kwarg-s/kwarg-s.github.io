---
title : "다익스트라 알고리즘"
excerpt : "dijkstra, greedy"
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

# 다익스트라 알고리즘 (최단 경로)

## 다익스트라 알고리즘

BFS(너비우선탐색)와 유사하다.

Minheap을 활용한다.

시간복잡도: O(E) + O(ElogE) = O(E+ElogE)=O(ElogE)

- 1단계: 각 노드마다 연결된 노드를 검사 = 노드간의 연결을 한번씩 검사해야함 = 엣지 개수만큼 검사해야함
- 2단계: 우선순위 큐에 엣지 개수 * 큐에 업데이트

## 다익스트라 알고리즘 설명

시작점에서 인접한 노드부터 보기 때문에 BFS와 유사한데,

시작점에서 특정 노드까지 가는 거리를 새로 제시된 경로의 거리와 비교하면서 가장 짧은 거리로 업데이트하는 것이다. 

1. 모든 노드에 대한 거리를 무한대로 초기화시켜놓고
2. 연결된 노드를 하나하나씩 체크하여 시작점에서 해당 노드까지 가는 거리를 측정한다. 그 거리가 짧으면 딕셔너리에 업데이트한다.
3. 추가적으로, 다음 노드의 우선순위를 체크하기 위해 우선순위큐에도 업데이트를 한다. 
4. 다음 노드는 우선순위큐 순으로 이동한다. 

## 다익스트라 예시

![/assets/img/algorithm/di.png](/assets/img/algorithm/di.png)

두 개의 데이터 구조를 마련한다: 딕셔너리(최종 거리), 우선순위 큐

- 딕셔너리는 다 inf로 채우고, 시작점만 0을 채워넣는다.
- 큐에는 시작점 [거리,노드]를 넣어둔다. 이제 큐에서 가장 우선적인 곳부터 볼 것이다.

[딕셔너리](https://www.notion.so/3908b190eabf484090ea440bfed07437)

[우선순위큐](https://www.notion.so/8bf5c97fbd7846288103c8e479894e42)

- 큐의 맨 앞인 [A,0]을 보기시작한다. 지금 보고 있는 [A,0]을 기준으로, 연결되어 있는 B,C,D를 하나씩 본다.
- A→B 경로의 합이 딕셔너리내 합보다 짧으면
    - 딕셔너리에 업데이트한다.
    - 우선순위 큐에 넣는다 (우선순위 순서대로 정렬됨)

```markdown
A->B=8 vs d[B]=inf  :  업데이트
A->C=1 vs d[C]=inf  :  업데이트
A->D=2 vs d[D]=inf  :  업데이트
```

[딕셔너리](https://www.notion.so/7e52c0ec693a4551bf53bcc08a43f110)

[우선순위큐](https://www.notion.so/4770073dcc364c599ef08388cf462a82)

- [C,1]을 보기시작한다. [C,1]을 기준으로 연결되어 있는 B,D를 본다.
- A→C→B 경로의 거리가 딕셔너리내 거리보다 짧으면
    - 딕셔너리에 짧은 거리를 업데이트한다.
    - 우선순위 큐에 짧은 거리와 노드를 넣는다 (우선 순위 순서대로 정렬됨)

```python
A->C->B=6 vs d[B]=8  :  업데이트
A->C->D=3 vs d[D]=2  :  업데이트 안함
```

[딕셔너리](https://www.notion.so/3672c4e28ab247e08217fbfbd7ff8704)

[우선순위큐](https://www.notion.so/a88a175415d44bd580b0d5ea17ea1373)

## 파이썬 코드 구현

```python
import heapq

def dijkstra(graph, start):
    
    #distances=딕셔너리=마지막 아웃풋
    #queue=우선순위큐
    
    #모든 경로를 inf로 채움
    distances = {node: float('inf') for node in graph}
    
    #시작점만 0으로 세팅
    distances[start] = 0
    
    #우선 순위 큐를 만듬
    queue = []
    
    #[우선순위=0, 시작점]을 넣는다.
    heapq.heappush(queue, [distances[start], start])
    
    #우선 순위가 텅빌때까지 반복
    while queue:
        
        #가장 우선 순위가 높은(거리가 짧은) 노드의 거리와 노드를 뽑는다.
        current_distance, current_node = heapq.heappop(queue)
        
        # 현재 저장단 익셔너리보다 큐의 거리가 더 크면 아무것도 하지 않음
        if distances[current_node] < current_distance:
            continue
            
        # 현재 저장된 딕셔너리보다 큐의 거리가 더 가까우면
        # 큐에 해당하는 노드와 접한 노드들을 가져와서
        for adjacent, weight in graph[current_node].items():
            
            # 현재의 거리와 그 노드와의 거리를 더한다
            distance = current_distance + weight
            
            # 더한 거리가 딕셔너리 내 그 노드와의 거리보다 짧으면 
            # 딕셔너리에 넣는다.&큐에 넣는다.
            if distance < distances[adjacent]:
                distances[adjacent] = distance
                heapq.heappush(queue, [distance, adjacent])
                
    return distances

mygraph = {
    'A': {'B': 8, 'C': 1, 'D': 2},
    'B': {},
    'C': {'B': 5, 'D': 2},
    'D': {'E': 3, 'F': 5},
    'E': {'F': 1},
    'F': {'A': 5}
}

dijkstra(mygraph, 'A')
```

### Reference

[https://www.fun-coding.org/Chapter18-dfs-live.html](https://www.fun-coding.org/Chapter18-dfs-live.html)