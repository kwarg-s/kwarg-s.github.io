---
title : "Prim 알고리즘"
excerpt : "greedy, prim, 프림, 최소신장트리, minimal spanning tree"
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

## Prim 알고리즘

시작 정점을 선택한 후 정점에 입접한 간선 중 최소 간선으로 연결된 정점을 선택하고, 해당 정점에서 다시 최소 간선으로 연결된 정점을 선택하는 방식

## 프림 알고리즘 프로세스

1. candidate 엣지 리스트=[](heapq이용), 
연결된 노드 집합={}
2. 아무 노드를 랜덤으로 선택합니다.
3. 현재 노드와 연결된 엣지들을 candidate 엣지 리스트에 넣습니다.
4. candidate 엣지 리스트에 있는 엣지들 중에서 가장 가중치가 낮은 것부터 pop
    1. 엣지와 연결된 노드가 연결된 노드 집합에 이미 있으면 스킵
    2. 노드 집합에 없으면 그 엣지와 연결된 노드를 연결된 노드 집합에 추가, 연결된 노드와 연결된 엣지 정보를 candidate 엣지 리스트에 push
5. candidate 엣지 리스트가 동날 떄까지 4번 반복

## 파이썬 구현

```python
from collections import defaultdict
from heapq import *

def prim(start_node, edges):
    
    #최소 신장 트리 : 트리에 들어가는 엣지 정보를 보관해서 리턴 // [(엣지정보),(엣지정보)]
    mst = list()
    
    #** 엣지 딕셔너리 : 전체 엣지 정보를 보관할 딕셔너리 // '노드':[(엣지정보),(엣지정보)]
    adjacent_edges = defaultdict(list)#빈공간은 리스트로
    
    #엣지 정보를 인접 엣지 딕셔너리에 추가한다. 
    for weight, n1, n2 in edges:
        adjacent_edges[n1].append((weight, n1, n2)) #'A': [(w,A,n2),(w,A,n2)....]
        adjacent_edges[n2].append((weight, n2, n1))
    
    #** 연결된 노드 집합 : 시작 노드를 세트로 만든다. 
    connected_nodes = set(start_node)
    
    #** candidate 엣지 리스트: 가중치 작은 것부터 정렬되는 리스트  
    candidate_edge_list = adjacent_edges[start_node] #인접한 엣지 정보를 리스트로 불러온 다음에
    heapify(candidate_edge_list) #heapq로 변환한다. 
    
    #candidate 엣지 리스트가 동날때까지
    while candidate_edge_list: 
        weight, n1, n2 = heappop(candidate_edge_list) #가장 가중치 낮은 노드 정보를 뽑은 후
        if n2 not in connected_nodes: #만약 n2(연결당하는 노드)가 노드 집합에 없으면 (있으면 싸이클 되므로 생략)
            connected_nodes.add(n2) #노드 집합에 넣은 후
            mst.append((weight, n1, n2)) #최소 신장 트리에 엣지 추가
            
            for edge in adjacent_edges[n2]: #n2와 연결된 엣지에 대해
                if edge[2] not in connected_nodes:#노드 집합에 없는 엣지에 대해서만
                    heappush(candidate_edge_list, edge) #candidate 엣지 리스트에 추가한다

    return mst

myedges = [
    (7, 'A', 'B'), (5, 'A', 'D'),
    (8, 'B', 'C'), (9, 'B', 'D'), (7, 'B', 'E'),
    (5, 'C', 'E'),
    (7, 'D', 'E'), (6, 'D', 'F'),
    (8, 'E', 'F'), (9, 'E', 'G'),
    (11, 'F', 'G')
]
prim ('A', myedges)

'''
[(5, 'A', 'D'),
 (6, 'D', 'F'),
 (7, 'A', 'B'),
 (7, 'B', 'E'),
 (5, 'E', 'C'),
 (9, 'E', 'G')]
'''
```

### Reference

- [https://www.fun-coding.org/Chapter20-prim-live.html](https://www.fun-coding.org/Chapter20-prim-live.html)