---
layout: post
title:  "[자료구조] 트리(Tree)"
subtitle:   "자료구조 트리 정리"
categories: dev
tags: Tree
comments: false
---

## 개요
> 자료구조 `트리(Tree)`에 대한 정리글입니다.

- 목차
	- [트리(Tree)란?](#트리(Tree)란?) 
    - [이진 탐색 트리(Binary Search Tree)란?](#이진-탐색-트리(Binary-Search-Tree)란?)
    - [이진 탐색 트리 구현](#이진-탐색-트리-구현)
    - [이진 탐색 트리 시간 복잡도](#이진-탐색-트리-시간-복잡도)

## 트리(Tree)란?
---

* __트리(Tree)__  
자료구조 트리는 자료구조 그래프의 하위 범위로 `노드(Node)`와 `브랜치(Branch)`를 이용해서, 사이클을 이루지 않도록 구성한 데이터 구조이다.

* __트리 기본 구조, 용어__  
![이미지1](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-18-dev-algorithm-tree-picture1.png) 
  - __트리 용어__  
  Node: 트리에서 데이터를 저장하는 기본 요소로 데이터와 연결된 노드의 브랜치 정보 포함  
  Root Node: 트리 맨 위에 있는 노드  
  Level: 최상위 노드를 Level 0으로 했을 때, 하위 브랜치로 연결된 노드의 깊이  
  Parent Node: 부모 노드 (어떤 노드가 파생된 바로위 상위 레벨 노드)  
  Child Node: 자식 노드 (어떤 노드로 부터 파생된 바로 하위 레벨 노드)  
  Leaf Node(Terminal Node): Child Node가 하나도 없는 노드  
  Sibling (Brother Node): 동일한 Parent Node를 가진 노드  
  Depth: 트리에서 Node가 가질 수 있는 최대 Level  


## 이진 탐색 트리(Binary Search Tree)란?
---
자료 구조 트리는 특히 이진 트리 형태로 탐색 알고리즘 구현에 많이 활용된다.  
> 이진 트리: 노드의 최대 브랜치가 2인 트리  

* __이진 탐색 트리란?__  
이진 탐색 트리(BST)는 왼쪽 노드는 해당 노드보다 작은 값, 오른쪽 노드는 해당 노드보다 큰 값을 가지게 유지한 이진 트리 구조이다.  

* __이진 탐색 트리의 장단점__  
  - 장점: 탐색 속도를 개선할 수 있어 `데이터 검색`에 효율적이다.  
  - 단점: 이진 탐색 트리를 유지시켜줘야 하며, 삭제 메서드가 비교적 복잡하다.


## 이진 탐색 트리 구현
---
이진 탐색 트리를 구현할 때, 삭제 기능에서 여러 상황이 나뉘어진다. 그만큼 삭제 메서드 코드를 짤 때 어려움을 겪을 수 있다. 따라서 삭제 기능에서 발생할 수 있는 다양한 상황을 나누어 생각해보며 기능을 구현해야 한다.  
삭제 기능을 구현했을 때 발생할 수 있는 여러 상황을 간략하게 요약, 필기해보았다. 더하여 상황에 맞게 어떻게 코드를 구현해야할지 구현 방향에 대해서도 노란색으로 필기해보았다.
![이미지2](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-18-dev-algorithm-tree-picture2.jpg) 

* __Leaf Node삭제__  
삭제할 노드의 부모 노드가 삭제할 노드를 가리키지 않도록 한다.(None을 가리키도록 한다.)  
```python
if  self.current_node.left == None and self.current_node.right == None: # 삭제할 노드의 오른쪽, 왼쪽 자식이 없는 경우
    if value < self.parent.value: # 삭제할 값이 부모 노드보다 작을 경우,
        self.parent.left = None # 부모 노드의 왼쪽 자식 값을 None으로 변경
    else: # 삭제할 값이 부모 노드보다 클 경우,
        self.parent.right = None # 부모 노드의 오른쪽 자식 값을 None으로 변경
    del self.current_node # 삭제할 노드를 메모리에서 삭제
```  

* __자식 노드가 1개인 노드 삭제__  
삭제할 노드의 부모 노드가 삭제할 노드의 자식 노드를 가리키도록 한다.  
```python
if self.current_node.left != None and self.current_node.right == None: # 삭제할 노드의 왼쪽 자식이 있는 경우
    if value < self.parent.value: # 삭제할 값이 부모 노드 보다 작다면, 
        self.parent.left = self.current_node.left # 부모 노드의 왼쪽을 삭제할 노드의 왼쪽 자식으로 변경
    else: # 삭제할 값이 부모 노드 보다 크다면,
        self.parent.right = self.current_node.left # 부모 노드의 오른쪽을 삭제할 노드의 왼쪽 자식으로 변경
elif self.current_node.left == None and self.current_node.right != None: # 삭제할 노드의 오른쪽 자식이 있는 경우
    if value < self.parent.value: # 삭제할 값이 부모 노드 보다 작다면,
        self.parent.left = self.current_node.right # 부모 노드의 왼쪽 값을 삭제할 노드의 오른쪽 자식으로 변경
    else: # 삭제할 값이 부모 노드 보다 크다면,
        self.parent.right = self.current_node.right # 부모 노드의 오른쪽 값을 삭제할 노드의 오른쪽 자식으로 변경
```  

* __자식 노드가 2개인 노드 삭제__  
자식 노드가 2개인 노드를 삭제하는 경우, 다음과 같은 2가지 방법이 있다. 두 방법 중에서 하나를 선택하여 구현한다. 아래의 구현 코드는 1번 방법을 선택하여 구현한 코드이다.  
> 1. 삭제할 노드의 오른쪽 자식 중에서 가장 작은 값을 삭제할 노드의 부모 노드가 가리키게함  
> 2. 삭제할 노드의 왼쪽 자식 중에서 가장 큰 값을 삭제할 노드의 부모 노드가 가리키게함  

```python
# 삭제할 노드가 부모 노드의 왼쪽에 있을 때,
if self.current_node.left != None and self.current_node.right != None: # case3: 삭제할 노드가 2개의 자식이 있는 경우
    if value < self.parent.value: # case3-1: 삭제할 노드 값이 부모 노드 보다 작을 경우, (부모의 왼쪽에 위치하는 경우)
        self.change_node = self.current_node.right # 삭제할 노드의 오른쪽에서 최속값을 탐색하기 위해 change_node 변수 선언.
        self.change_node_parent = self.current_node.right # change_node의 부모 값을 알아야 트리 연결이 가능
        while self.change_node.left != None: # 최소값을 찾는다.
            self.change_node_parent = self.change_node
            self.change_node = self.change_node.left
        if self.change_node.right != None: # 최소값의 오른쪽 자식이 있다면
            self.change_node_parent.left = self.change_node.right # 현재 변경하려는 최소값과 연결된 부모 왼쪽 지점을 최소값의 오른쪽 자식으로 연결
        else: # 최소값의 오른쪽 자식이 없다면
            self.change_node_parent.left = None # 최소값의 부모의 왼쪽(현재 변경하려는 최소값 위치)에 None을 넣는다.
        self.parent.left = self.change_node # 찾은 최소값 지점을 삭제하려는 노드의 부모 왼쪽에 연결
        self.change_node.right = self.current_node.right # 변경한 최소값의 왼쪽 오른쪽을 삭제 노드(current node)의 왼쪽, 오른쪽으로 연결해준다.
        self.change_node.left = self.change_node.left

# 삭제할 노드가 부모 노드의 오른쪽에 있을 때,
# 위의 조건문(삭제할 지점이 부모 노드로 부터 왼쪽이냐 오른쪽이냐를 구분하는 조건문)에 이어서 
else: # 만약 삭제할 노드의 값이 부모 노드 보다 크다면(부모 노드 보다 오른쪽에 위치한다면)
    self.change_node = self.current_node.right # 삭제 지점 노드로 부터 오른쪽 트리에서 최소값을 찾기 위해 change_node 변수 선언
    self.change_node_parent = self.current_node.right # change_node의 부모 노드 또한 알아야 바꿀 최소값 지점을 메꿔 트리를 유지할 수 있다. 
    while self.change_node.left != None: # 최소값 지점을 찾을 때 까지 while문 반복
        self.change_node_parent = self.change_node
        self.change_node = self.change_node.left
    if self.change_node.right != None: # 최소값 지점의 오른쪽 자식이 있다면,
        self.change_node_parent.left = self.change_node.right # 최소값 지점의 부모 노드의 왼쪽(즉, 현재 최소값 노드의 위치)에 최소값 노드의 오른쪽 자식을 넣음
    else: # 최소값 지점의 오른쪽 자식이 없다면
        self.change_node_parent.left = None # 최소값 지점의 부모 노드의 왼쪽(현재 최소값 노드의 위치)에 None을 넣음
    self.parent.right = self.change_node # 삭제 지점(부모 노드의 오른쪽)에 찾은 최소값을 넣음
    self.change_node.left = self.current_node.left # 넣은 최소값 오른쪽, 왼쪽을 본래 삭제지점이 갖고 있었던 오른쪽, 왼쪽 노드로 연결
    self.change_node.right = self.current_node.right
```  

* __이진 탐색 트리 구현__  
위의 삭제 기능을 기반으로한 이진 탐색 트리를 구현한 코드이다.  
```python
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

        
class NodeMgmt:
    def __init__(self, head):
        self.head = head
    
    def insert(self, value):
        self.current_node = self.head
        while True:
            if value < self.current_node.value:
                if self.current_node.left != None:
                    self.current_node = self.current_node.left
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
        elif self.current_node.left != None and self.current_node.right != None:
            # case3-1
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
                self.change_node.right = self.current_node.right
                self.change_node.left = self.current_node.left

        return True
```  


## 이진 탐색 트리 시간 복잡도
---
이진 탐색 트리는 한 level 마다 경우의 수가 절반씩 줄어드는 구조로 `O(logN)`의 시간 복잡도를 갖는다고 할 수 있다.  
하지만, 이는 이진 탐색 트리의 구조가 균형이 잡혀있을 때의 시간 복잡도이다. 만약, 이진 탐색 트리의 왼쪽이나 오른쪽으로 노드가 치우친 구조일 경우에는 효율성이 보다 떨어지게 되며, 최악의 경우에는 연결 리스트와 동일한 직선인 형태로 O(N)의 시간 복잡도를 갖는다.