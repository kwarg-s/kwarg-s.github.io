---
title : "너비 우선 탐색 BFS"
excerpt : "Breath First Search"
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

# 너비 우선 탐색 (BFS)

## 너비 우선 탐색 Breath First Search

정점들과 같은 레벨에 있는 노드들 (형제 노드들)을 먼저 탐색하는 방식

시간복잡도: O(노드+엣지)

```python
def bfs(graph, start_node):
    
    visited, need_visit = list(), list() #(이미 가 본 노드), (곧 가야하는 노드)
    need_visit.append(start_node) #스타트 노드에서 시작하므로 (곧 가야하는 노드=스타트노드)
    
    while need_visit: #바닥날 떄까지
        node = need_visit.pop(0) #방문한 노드를 pop한다. (첫 노드부터 방문한다)
        #첫 노드인 이유: 형제들이 앞에 있고, 자식들은 뒤에 있음
        
        if node not in visited:
            visited.append(node)
            need_visit.extend(graph[node])
    
    return visited

bfs(graph, 'A') 
#['A', 'B', 'C', 'D', 'G', 'H', 'I', 'E', 'F', 'J']
```