---
title : "이진 탐색"
excerpt : "divide, conquer, 이진 탐색, 이진 탐색 트리"
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

# 이진 탐색

## 이진 탐색 Binary search

탐색할 자료를 둘로 나누어 해당 데이터가 있을만한 곳을 탐색하는 방법

시간복잡도: O(logn)

### Divide & conquer

- Divide: 문제를 나눈다.
- Conquer: 나눠진 문제가 충분히 작고 해결이 가능하다면 해결한다, 문제가 너무 크거나 해결이 어렵다면 다시 나눈다.

### 이진 탐색

- Divide: 리스트를 둘로 나눈다
- Conquer: 검색하는 숫자가 중간값보다 크면 두번째 리스트에서 찾는다. / 작으면 첫번째 리스트에서 찾는다.

```python
def binary_search (data, search):
    
    #리스트에 데이터 1개밖에 없으면, 그 데이터가 맞는지만 확인하기
    if len(data) == 1:
        if data[0] == search:
            return True
        else:
            return False
    
    #리스트가 비어있으면 없음
    if len(data) == 0:
        return False
    
    #리스트가 2개 이상이면, 일단 데이터를 둘로 나눈다. 
    medium = len(data) // 2
    
    #중앙값(data[medium])과 일치하면, 트루
    if search == data[medium]:
        return True
    #중앙값보다 크면 뒷부분에서 계속 찾기
    if search > data[medium]:
        return binary_search(data[medium:], search)
    #중앙값보다 작으면 앞부분에서 계속 찾기
    else:
        return binary_search(data[:medium], search)
```

```python
import random 
data_list = random.sample(range(100), 11)
data_list.sort()
binary_search(data_list, 50)
```

## 이진 탐색 트리 Binary search tree

왼쪽 노드는 작은값, 오른쪽 노드는 큰값

시간복잡도 : 평균 O(logn) ~ 최대O(n)

```python
class BST:
    def __init__(self, node):
        #생성할때 헤드노드에서 시작
        self.head = node
    
    def insert(self, value):
        #헤드에서 시작
        self.current_node = self.head
        while True:
            #새 값이 현재보다 작고
            if value < self.current_node.value:
                #왼쪽 자식이 있으면-->왼쪽 자식으로 현재 노드를 바꾼후 루프 돌림
                if self.current_node.left != None:
                    self.current_node = self.current_node.left
                #왼쪽 자식이 없으면-->왼쪽 자식으로 삽입함
                else:
                    self.current_node.left = Node(value)
                    break
            else:
                if self.current_node.right != None:
                    self.current_node = self.current_node.right
                else:
                    self.current_node.right = Node(value)
                    break
    
    def search(self, value):
        #헤드에서 시작
        self.current_node = self.head
        while self.current_node:
            if self.current_node.value == value:
                return True
            elif value < self.current_node.value:
                self.current_node = self.current_node.left
            else:
                self.current_node = self.current_node.right
        return False
```