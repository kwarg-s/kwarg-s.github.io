---
title : "연결리스트, 해쉬"
excerpt : "Linked list, hash"
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

# 링크드 리스트, 해쉬

## Linked List

떨어진 곳에 존재하는 데이터를 링크로 연결하여 관리함

- 데이터 공간 할당 필요 엇음
- 속도가 느림. 삭제 시 재구성 해야함

```python
class LinkedList:
    def __init__(self):
        self.head=None
    
    def add(self,item):
        #새노드생성
        new_node=Node(item)
        #첫노드인경우
        if self.head==None:
            self.head=new_node
        #첫노드 아닌경우
        else:
            this_node=self.head
            while this_node.next!=None:
                this_node=this_node.next
                #this_node.next가 비어있을 때까지
            this_node.next=new_node
            #비어있는 다음 노드에 새 노드 삽입
            
    def delete(self, item): 
        node = self.head 
        if node.item == data: 
            self.head = node.next 
            del node 
        else: 
            while node.next: 
                next_node = node.next 
                if next_node.item == data: 
                    node.next = next_node.next 
                    del next_node 
                else: 
                    node = node.next 
    
    def find(self, item): 
        node = self.head 
        while node: 
            if node.item == item: 
                return node 
            else: 
                node = node.next

#출처: https://davinci-ai.tistory.com/
```

## 해쉬

key와 value로 이루어진 데이터 구조이다. 

- 검색이 빠르다.
- 저장공간 필요.
- 시간복잡도: 1 ~ n

### Python의 dictionary

```python
d={1:'a',2:'b',3:[1,2,3]}
del a[2]
a.keys()
a.items()
a.get(1)
a.clear()
```

