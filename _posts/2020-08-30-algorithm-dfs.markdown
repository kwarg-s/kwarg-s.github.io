---
title : "깊이 우선 탐색 DFS"
excerpt : "Depth First Search"
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

# 깊이 우선 탐색 (DFS)

## 그래프

- 노드=vertex
- 엣지=link=branch
- degree: 하나의 노드와 연결된 노드의 수
- in-degree, out-degree

```python
graph = dict()

graph['A'] = ['B', 'C']
graph['B'] = ['A', 'D']
graph['C'] = ['A', 'G', 'H', 'I']
graph['D'] = ['B', 'E', 'F']
graph['E'] = ['D']
graph['F'] = ['D']
graph['G'] = ['C']
graph['H'] = ['C']
graph['I'] = ['C', 'J']
graph['J'] = ['I']
```

## 깊이 우선 탐색 Depth First Search

정점의 자식들을 먼저 탐색하는 방식

시간복잡도: O(노드수+엣지수)

![/assets/img/algorithm/dfsbfs.jpg](/assets/img/algorithm/dfsbfs.jpg)

```python
def dfs(graph, start_node):
    
    visited, need_visit = list(), list() #(이미 가 본 노드), (곧 가야하는 노드)
    need_visit.append(start_node) #스타트 노드에서 시작하므로 (곧 가야하는 노드=스타트노드)
    
    while need_visit: #곧 가야할 노드 목록이 바닥날 때까지
        node = need_visit.pop() #방문한 노드를 pop한다. (마지막 노드부터 방문한다)
        if node not in visited: #아직 안 가 본 노드일 경우
            visited.append(node) #방문하고 방문한 목록에 추가한다.
            need_visit.extend(graph[node]) #곧 가야할 노드 목록에 내가 방문한 노드와 연결된 곳을 다 추가한다.
    return visited

dfs(graph, 'A')
```

### Reference

[https://www.fun-coding.org/Chapter18-dfs-live.html](https://www.fun-coding.org/Chapter18-dfs-live.html)