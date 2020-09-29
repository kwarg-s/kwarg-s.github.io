---
title : "트리"
excerpt : "이진트리, 이진탐색트리"
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

# 트리

## 트리란

node, branch를 이용해서 사이클을 이루지 않도록 구성한 데이터 구조

### 트리 용어

1. 노드
2. 로트 노드
3. 레벨
4. 부모노드,자식노드
5. leaf 노드: 자식이 하나도 없는 노드

### 이진 트리

노드의 최대 브랜치가 2인 트리

### 이진 탐색 트리

Binary Search Tree (BST)

왼쪽 노드는 부모보다 작은 값

오른쪽 노드는 부모보다 큰 값

- 장점: 탐색 속도 개산 가능

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
           
    def delete(self, value):
        # 삭제할 노드 탐색
        searched = False
        self.current_node = self.head
        self.parent = self.head
        while self.current_node:
            if self.current_node.value == value:
                searched = True
                break
            elif value < self.current_node.value:
                self.parent = self.current_node
                self.current_node = self.current_node.left
            else:
                self.parent = self.current_node
                self.current_node = self.current_node.right

        if searched == False:
            return False    

        # case1
        if  self.current_node.left == None and self.current_node.right == None:
            if value < self.parent.value:
                self.parent.left = None
            else:
                self.parent.right = None
        
        # case2
        elif self.current_node.left != None and self.current_node.right == None:
            if value < self.parent.value:
                self.parent.left = self.current_node.left
            else:
                self.parent.right = self.current_node.left
        elif self.current_node.left == None and self.current_node.right != None:
            if value < self.parent.value:
                self.parent.left = self.current_node.right
            else:
                self.parent.right = self.current_node.right        
        
        # case 3
        elif self.current_node.left != None and self.current_node.right != None: # case3
            if value < self.parent.value:
                self.change_node = self.current_node.right
                self.change_node_parent = self.current_node.right
                while self.change_node.left != None:
                    self.change_node_parent = self.change_node
                    self.change_node = self.change_node.left
                if self.change_node.right != None:
                    self.change_node_parent.left = self.change_node.right
                else:
                    self.change_node_parent.left = None
                self.parent.left = self.change_node
                self.change_node.right = self.current_node.right
                self.change_node.left = self.change_node.left
            # case 3-2
            else:
                self.change_node = self.current_node.right
                self.change_node_parent = self.current_node.right
                while self.change_node.left != None:
                    self.change_node_parent = self.change_node
                    self.change_node = self.change_node.left
                if self.change_node.right != None:
                    self.change_node_parent.left = self.change_node.right
                else:
                    self.change_node_parent.left = None
                self.parent.right = self.change_node
                self.change_node.left = self.current_node.left
                self.change_node.right = self.current_node.right

        return True
```

```python
head = Node(1) #헤드 노드 만든다음
binary_tree = BST(head) #헤드 노드에서 시작하는 트리 만듬
binary_tree.insert(2)
binary_tree.insert(0)
binary_tree.insert(4)
binary_tree.insert(8)
binary_tree.insert(-2)
binary_tree.search(8)
```

### Reference

[https://www.fun-coding.org/Chapter10-tree.html](https://www.fun-coding.org/Chapter10-tree.html)