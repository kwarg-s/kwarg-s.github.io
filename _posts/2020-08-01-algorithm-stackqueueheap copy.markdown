---
title : "스택, 큐, 힙"
excerpt : "Stack, queue, heap"
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

# 스택,큐,힙

## 스택

### 스택

뒤로만 넣고 뺄 수 있는 연결 리스트이다.

- 단순, 빠름
- 공간 낭비—> 그냥 배열을 쓴다.

### 스택 만들어 보기

```python
class Stack:
    def __init__(self):
        self.data=[]
    def __init__(self,data):
        self.data=data
    def push(self,item):
        self.data.append(item)
    def pop(self,item):
        if self.data==[]:
            return -1
        temp=self.data[-1]
        self.data=self.data[:-1]
        return temp
    def print(self):
        print(self.data)
```

## 큐

뒤에서 들어가고 앞에서 빠지는 구조(FIFO)

### 큐 만들어 보기

```python
class Queue:
    def __init__(self):
        self.queue=[]
    def __init__(self,data):
        self.queue=data
    def dequeue(self):
        if self.queue==[]:
            return -1
        temp=self.queue[0]
        self.queue=self.queue[1:]
        return temp
    def enqueue(self,item):
        self.queue.append(item)
    def print(self):
        print(self.queue)
```

### collections.deque()

```python
from collections import deque

dq=deque([1,2,3])
dq.appendleft(0) #왼쪽
dq.append(4) #오른쪽
dq.popleft() #왼쪽
dq.pop() #오른쪽

```

### queue.Queue()

```python
import queue
q=queue.Queue()
q.put(1)
q.put(2)
q.get()#1
```

### queue.LifoQueue()

```python
import queue
q=queue.LifoQueue()
q.put(1)
q.put(2)
q.get()#2
```

### queue.PriorityQueue()

```python
import queue
q=queue.PriorityQueue()
q.put((10,'A')) #(priority,item)
q.put((2,'B')) #(priority,item)
q.put((5,'C')) #(priority,item)
q.get() #(2,'B')
```

## 힙이란

트리의 일종으로, "완전이진트리"를 기본으로 한 자료구조. 

1. 최대 힙: 부모 노드의 키 값이 자식 노드의 키 값보다 항상 크다.
2. 최소 힙: 부모 노드의 키 값이 자식 노드의 키 값보다 항상 작다.

시간 복잡도는 O(log n)

참고: [https://koreanfoodie.me/108](https://koreanfoodie.me/108) 

**insert**

맨 마지막에 넣고 나서 부모랑 비교한다. 부모보다 크면 위로 올려보낸다. 부모보다 안클때까지 한다.

**delete + sort_maxheap**

가장 큰 수에 해당하는 root를 리턴하면서 지운다. root(idx=0)가 공란이 되면 맨 마지막 숫자를 root에 놓는다. 이번에는 자식이랑 비교하면서 자식보다 작으면 아래로 내려보낸다. 자식보다 안작을 때까지 한다. 자식이 2개이기 때문에, 나/자식1/자식2 셋을 비교해서 가장 큰 숫자를 꼭대기에 놓는다. 

```python
class MaxHeap:
    def __init__(self):
        self.data=[]
    
    ###필요한 함수###
    def swap(self,idx,parent_idx):#자리 바꾸는 함수
        self.data[idx],self.data[parent_idx]=\
        self.data[parent_idx],self.data[idx]
    
    def parent(self,idx):
        return (idx-1)//2
    
    def leftchild(self,idx):
        return idx*2+1
    
    def rightchild(self,idx):
        return idx*2+2
    
    ###insert,delete,sort###
    def insert(self,item):
        self.data.append(item)
        
        #heap sort
        idx=len(self.data)-1
        while idx>=0:
            parent_idx=self.parent(idx) #내가 있는 곳의 부모
            #부모님보다 내가 더 큰 경우
            if parent_idx>=0 and\
                self.data[parent_idx]<self.data[idx]:
                self.swap(idx,parent_idx) #나와 부모의 자리를 바꾼다.
                idx=parent_idx
            else: #더 이상 내가 크지 않으면 멈춘다.
                break
    
    def delete(self):
        idx=len(self.data)-1
        if idx<0:
            return -1
        self.swap(0,idx)#자리를 바꾼후
        max_number=self.data.pop()#마지막 원소를 제거
        self.sort_maxheap(0)
        print(max_number)
        return max_number#제거했던 마지막 원소를 리턴
    
    def sort_maxheap(self,idx): #idx부터 자식들을 정렬함
        left_idx=self.leftchild(idx)
        right_idx=self.rightchild(idx)
        max_idx=idx#시작 idx
        
        if left_idx<=len(self.data)-1 and\
        self.data[max_idx]<self.data[left_idx]:
            max_idx=left_idx
            
        if right_idx<=len(self.data)-1 and\
        self.data[max_idx]<self.data[right_idx]:
            max_idx=right_idx
        
        if max_idx!=idx: #top idx가 가장 큰수가 아니면
            self.swap(idx,max_idx) #자리바꾼후
            self.sort_maxheap(max_idx) #재귀정렬
```

```python
mh=MaxHeap()
mh.insert(16)
mh.insert(14)
mh.insert(10)
mh.insert(8)
mh.insert(7)
mh.insert(9)
mh.insert(3)
mh.insert(2)
mh.insert(4)
mh.insert(1)
mh.data #[16, 14, 10, 8, 7, 9, 3, 2, 4, 1]
mh.delete() #16 
mh.delete() #14
mh.delete() #10
mh.delete() #9
mh.delete() #8
mh.delete() #7
mh.delete() #4
mh.delete() #3
mh.delete() #2
mh.delete() #1
```

## Heapq 라이브러리 (min heap)

- heapq.heappush(lst,n) #lst라는 힙에 b를 넣음
- heapq.heappop(lst) #배열에서 최소값 꺼냄

```python
#최소 힙
import heapq
heap = []
heapq.heappush(heap, 4)
heapq.heappush(heap, 1)
heapq.heappush(heap, 7)
heapq.heappush(heap, 3)
print(heap) #[1, 3, 7, 4]
```

```python
#최대 힙
import heapq
nums = [4, 1, 7, 3, 8, 5]
heap = []
for num in nums:
  heapq.heappush(heap, (-num, num))  # (우선 순위, 값
```

### Reference

- [https://www.daleseo.com](https://www.daleseo.com/python-heapq/)
- [https://davinci-ai.tistory.com/](https://davinci-ai.tistory.com/)
- [https://www.zerocho.com/](https://www.zerocho.com/)
- [https://koreanfoodie.me/108](https://koreanfoodie.me/108)