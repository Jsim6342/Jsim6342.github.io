---
layout: post
title:  "[자료구조] 연결 리스트(linked list)"
subtitle:   "자료구조 연결 리스트 정리"
categories: dev
tags: linked list
comments: false
---

## 개요
> 자료구조 `연결 리스트(linked list)`에 대한 정리글입니다.

- 목차
	- [연결 리스트(linked list)란?](#연결-리스트linked-list란?) 
    - [연결 리스트 장단점](#연결-리스트-장단점)
    - [연결 리스트 구현](#연결-리스트-구현)

## 연결 리스트(linked list)란?
---

* __연결 리스트(linked list)__  
일반적으로 여러 개의 데이터를 저장하기 위해서 배열을 사용한다. 배열을 사용하기 위해서는 우선적으로 배열을 선언해야 할텐데, 이 때 배열의 크기를 정해줘야 한다. 여기서 한 가지 문제점이 있다. '내가 데이터를 얼마나 넣을지 모르는 상황이라면 크기를 어떻게 정해줘야하지?'. 이러한 문제점을 해결하고자 고안된 것이 바로 연결 리스트다.

* __연결 리스트 기본 구조, 용어__  
연속된 공간에 데이터를 나열하여 저장하는 배열과는 달리, 연결 리스트는 떨어진 곳에 존재하는 데이터를 `포인터`라는 주소를 가리키는 변수에 담아 연결시킨 구조이다. 따라서, 배열의 크기를 정하지 않더라도 공간이 필요할 때 마다 포인터로 공간을 이어 여러 개의 데이터를 이어 저장할 수 있는 것이다.
> 연결 리스트 기본 구조와 용어  
> 노드(Node): 데이터 저장 단위이다. 데이터값과 포인터로 구성되어 있다.  
> 포인터(pointer): 각 노드 안에서, 다음이나 이전의 노드와의 연결 정보(주소값)을 가지고 있는 공간

* __연결 리스트 종류__  
연결 리스트에서 노드와 노드가 연결되어 있는 구조에 따라 단방향 연결 리스트와 양방향 연결 리스트가 있다.
  - `단방향 연결 리스트`는 첫 번째 head 노드에서 마지막 노드까지 다음 주소값만을 저장하여 이루어진 형태로 이전 노드 값에 대한 주소값이 없기 때문에 정해진 방향으로만 탐색을 할 수 있다.
  - `양방향 연결 리스트`는 한 노드에서 이전 노드와 다음 노드의 주소값을 모두 갖고 있다. 때문에 head와 tail 어디든지 탐색 방향을 선택하여 연결 리스트에 있는 데이터를 탐색할 수 있다.
  - 양방향 연결 리스트의 경우 이전 노드에 대한 주소값도 저장해야 하므로 저장 공간이 단방향 연결 리스트 보다는 크다고 할 수 있다. 하지만, 양방향 연결 리스트는 노드의 양쪽을 모두 탐색할 수 있기 때문에 데이터를 탐색하는 작업에 있어서는 보다 효율적으로 활용할 수 있다.

## 연결 리스트 장단점
---
배열과 비교하여 연결 리스트에는 장단점이 있다. 상황에 따라 적절한 자료구조를 사용하면 된다.  

* __장점__  
  - 데이터 공간을 미리 할당하지 않아도 된다.

* __단점__  
  - 포인터 저장 공간이 별도로 필요하므로 일반 배열에 비해 저장공간 효율이 비효율적
  - 데이터 탐색 시, 포인터를 따라 탐색을 진행하기 때문에 접근 속도가 배열에 비해 느리다.


## 연결 리스트 구현
---

* __파이썬으로 연결 리스트 구현__  
파이썬으로 간단하게 단방향 연결 리스트를 구현해보자.

```python
class Node:
    def __init__(self, data, next=None):
        self.data = data
        self.next = next
class linked_list:
    def __init__(self, data): # 연결 리스트 생성자
        self.head = Node(data)

    def add(self, data): # 연결 리스트 가장 뒤에 데이터 추가
        if self.head == '':
            self.head = Node(data)
        else:
            node = self.head
            while node.next:
                node = node.next
            node.next = Node(data)

    def select(self, data): # 특정 데이터 주소값 검색
        node = self.head

        while node:
            if node.data == data:
                return node
            else:
                node = node.next

    def delete(self, data): # 특정 데이터 삭제
        if self.head == "":
            print("해당 값을 가진 노드가 없습니다.")
            return
        
        if self.head.data == data:
            tmp = self.head
            self.head = self.head.next
            del tmp
        else:
            node = self.head
            while node.next: #node가 아니라 node.next를 기준으로 확인
                if node.next.data == data:
                    tmp = node.next
                    node.next = node.next.next
                    del tmp
                    return
                else:
                    node = node.next

    def desc(self):
        node = self.head
        while node:
            print(node.data)
            node = node.next
```
  -`delete함수`에서 node가 아니라 node.next를 기준으로 확인하는 이유는 node를 기준으로 확인할 경우, 현재 비교하는 node가 삭제할 데이터일 경우 바로 전 주소값에서 현재 node 주소값을 건너뛰고 다음 주소값으로 node를 연결해야하는 작업에서 바로 전 node의 next를 불러올 수 없다.  
  -`select함수`를 활용하여 연결 리스트 중간에 특정한 값의 주소값을 얻을 수 있다. 이를 활용하여 연결 리스트 중간에 값을 넣을 수도 있다.