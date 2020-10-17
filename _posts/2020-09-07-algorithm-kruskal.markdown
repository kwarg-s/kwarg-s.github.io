---
title : "Kruskal 알고리즘"
excerpt : "greedy, kruskal, 크루스칼, 최소신장트리, minimal spanning tree"
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

## 신장 트리 Spanning tree

- 모든 노드를 포함함
- 모든 노드가 연결됨
- 사이클이 없음

## 최소 신장 트리 minimal spanning tree

가능한 신장 트리 중에서 가중치의 합이 가장 작은 신장 트리

## 크루스칼 알고리즘 Kruskal algorithm

탐욕 알고리즘을 바탕으로 한다.

- 탐욕 알고리즘: 결정을 해야할 때마다 그 순간에 가장 좋다고 생각되는 것을 선택하는 것. 단점은 전체적 관점에서 최적이라는 보장이 없다는 것.

크루스칼 알고리즘은 최소 신장 트리를 찾는 방법이다.

1. 모든 엣지들을 오름차순으로 정렬한다.
2. 가중치가 낮은 엣지부터 순서대로 고른다.
3. 사이클을 형성하면 그 엣지는 버린다.
4. 엣지 개수가 노드수-1에 도달하면 멈춘다.

## Union find 알고리즘

크루스칼 알고리즘에서 네트워크가 사이클을 형성하는지의 여부를 확인하는 알고리즘

- Disjoint set: 서로 중복되지 않는 부분 집합들로 나눠진 원소들에 대한 정보를 저장하고 조작하는 자료구조

공통 원소가 없는 (서로소) 상호 배타적인 부분 집합들로 나눠진 원소들에 대한 자료구조를 의미함

- 프로세스
    1. parent딕셔너리 (마지막은 루트), rank딕셔너리 초기화
    2. 핵심은 두 노드의 루트 노드가 서로 같냐 다르냐이다. (find)
        1. 루트 노드가 서로 같으면 → 싸이클 형성할 것이므로 연결 하면 안됨
        2. 루트 노드가 서로 다르면 → 연결해도됨
    3. 다음 관문은 직접 연결하는 것인데 이때 적절하게 parent와 rank를 업데이트 해야함 (union)
        1. parent는 두 루트 노드 중 랭크가 더 높은 애를 새 parent로 하면된다.
        2. 두 루트 노드 랭크 차이가 이미 나면 그냥 두면 된다. 그런데 만약, 두 루트 노드가 차이가 안난다면 두 루트 노드 중 아무거나 선택해서 새 parent로 한다음 새 parent가 된 루트 노드의 랭크를 1 높인다.

- 핵심 함수
    - union: 두개의 트리를 하나의 트리로 연결하는 하는 함수
    - find: 두 개의 노드가 같은 그래프에 속하는지를  판별하기 위해 두 노드의 루트 노드를 찾는 함수

- path compression 기법: 두 노드의 parent가 같은지  확인한다 (find)
    - parent가 같음 → 두 노드를 연결하면 사이클이 된다. 그러므로 연결하면 안된다.
    - parent가 다름→ 연결해도된다.

- union by rank기법: 각 트리의 높이를 기억해둠. 두 네트워크를 연결하는 방법 (union)
    - 두 네트워크의 루트를 찾는다.
    - 두 루트의 랭크를 서로 비교한다
    - 낮은 랭크의 루트를 높은 랭크의 루트 아래에 연결한다

## 크루스칼 파이썬 구현

```python
parent = dict()
rank = dict()

def find(node):
    # path compression 기법 
    if parent[node] != node: #그노드의 부모가 자기자신이 아니면 (그 노드가 루트가 아니면)
        parent[node] = find(parent[node]) #그노드의 부모에 그노드의 부모의 부모를 대입
    return parent[node] #그노드의 루트를 리턴

def union(node_v, node_u):
    root1 = find(node_v) #그 노드의 루트
    root2 = find(node_u) #그 노드의 루트
    
    # union-by-rank 기법
    if rank[root1] > rank[root2]: #루트1의 랭크가 높으면
        parent[root2] = root1  #낮은루트2의 부모로 높은루트1을 설정한다
    else: #루트2의 랭크가 높으면
        parent[root1] = root2 #낮은루트1의 부모로 높은루트2를 설정한다 
        if rank[root1] == rank[root2]: #만약 두 루트의 랭크가 같았다면
            rank[root2] += 1 #루트2(높은루트)의 랭크를 더 높인다.
    
    
def make_set(node):
    parent[node] = node #루트로 설정(부모가 자기자신)
    rank[node] = 0 #랭크는 0으로 초기화

def kruskal(graph):
    mst = list()
    
    # 1. 초기화: 모든 노드를 초기화 (모든 노드를 루트로 설정)
    for node in graph['vertices']:
        make_set(node)
    
    # 2. 간선 weight 기반 sorting : 작은 weight부터 정렬한다.
    edges = graph['edges']
    edges.sort()
    
    # 3. 간선 연결 (사이클 없는)
    for edge in edges:
        weight, node_v, node_u = edge #가중치,노드1,노드2
        if find(node_v) != find(node_u): #두 노드의 루트가 서로 다른 경우에만 연결 가능. 
            union(node_v, node_u) #두 노드를 연결한 후
            mst.append(edge) #빈 리스트에 그 엣지를 추가한다. 
    
    return mst

mygraph = {
    'vertices': ['A', 'B', 'C', 'D', 'E', 'F', 'G'],
    'edges': [
        (7, 'A', 'B'),
        (5, 'A', 'D'),
        (7, 'B', 'A'),
        (8, 'B', 'C'),
        (9, 'B', 'D'),
        (7, 'B', 'E'),
        (8, 'C', 'B'),
        (5, 'C', 'E'),
        (5, 'D', 'A'),
        (9, 'D', 'B'),
        (7, 'D', 'E'),
        (6, 'D', 'F'),
        (7, 'E', 'B'),
        (5, 'E', 'C'),
        (7, 'E', 'D'),
        (8, 'E', 'F'),
        (9, 'E', 'G'),
        (6, 'F', 'D'),
        (8, 'F', 'E'),
        (11, 'F', 'G'),
        (9, 'G', 'E'),
        (11, 'G', 'F')
    ]
}

kruskal(mygraph)

```
[(5, 'A', 'D'),
 (5, 'C', 'E'),
 (6, 'D', 'F'),
 (7, 'A', 'B'),
 (7, 'B', 'E'),
 (9, 'E', 'G')]
```
```

### Reference

- [https://www.fun-coding.org/Chapter20-kruskal-live.html](https://www.fun-coding.org/Chapter20-kruskal-live.html)
- [https://gmlwjd9405.github.io/2018/08/29/algorithm-kruskal-mst.html](https://gmlwjd9405.github.io/2018/08/29/algorithm-kruskal-mst.html)
- [https://brownbears.tistory.com/461](https://brownbears.tistory.com/461)