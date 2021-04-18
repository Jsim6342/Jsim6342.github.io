---
layout: post
title:  "[자료구조] 힙(Heap)"
subtitle:   "자료구조 힙 정리"
categories: dev
tags: Heap
comments: false
---

## 개요
> 자료구조 `힙(Heap)`에 대한 정리글입니다.

- 목차
	- [힙(Heap)이란?](#힙(Heap)이란?) 
    - [이진 탐색 힙(Binary Search Heap)](#이진-탐색-힙(Binary-Search-Heap))
    - [이진 탐색 힙 구현](#이진-탐색-힙-구현)
    - [이진 탐색 힙 시간 복잡도](#이진-탐색-힙-시간-복잡도)

## 힙(Heap)이란?
---

*__힙(Heap)__  
`최대값`과 `최소값`을 빠르게 찾기 위해 고안된 `완전 이진 트리`
힙에는 최대값을 구하기 위한 `최대힙(Max Heap)`과 최소값을 구하기 위한 `최소힙(Min Heap)`이 있다.
> 완전 이진 트리(Complete Binary Tree): 노드를 삽입할 때 최하단 왼쪽 노드 부터 차례대로 삽입하는 트리

 - __힙 구조__  
 1. 각 노드의 값은 해당 노드의 자식 노드가 가진 값 보다 크거나 같다. (최대힙의 경우)  
 2. 완전 이진 트리 형태를 갖는다.


## 힙 구현
---
*__힙 동작__  
 -__삽입__  
 1) 힙은 완전 이진 트리의 구조 유지를 위해 삽입한 노드는 기본적으로 왼쪽 최하단부 노드 부터 순차적으로 채워지는 형태로 구현  
 2) 만약 채워진 노드가 부모 노드 보다 크다면(최대힙인 경우), 부모 노드와 자식 노드를 바꿔주는 작업을 반복해서 해준다.

 -__삭제__  
 힙의 용도는 최대값, 최소값을 빠르게 구하기 위한 자료구조로 힙의 삭제는 보통 최상단 노드(root 노드)를 의미한다. (최상단 노드의 값을 삭제함과 동시에 반환한다.)  
 1) 힙의 최상단 노드를 삭제한다.  
 2) 삭제한 최상단 노드에 마지막에 위치한 노드를 넣는다.  
 3) 최소 or 최대힙의 구조가 구현되도록 자식 노드와 부모 노드를 비교하며 자리를 바꿔준다.  

*__힙 동작__  
완전 이진 트리 구조를 배열을 통해 구현할 수 있다. 힙 구현의 편의를 위해 0번 인덱스에 None을 추가하고, root 노드 인덱스가 1인 배열을 만들어 최대힙을 구현해보자.
> 부모 노드 인덱스 번호  = 자식 노드 인덱스 번호 // 2  
> 왼쪽 자식 노드 인덱스 번호 = 부모 노드 인덱스 번호 * 2  
> 오른쪽 자식 노드 인덱스 번호  = 부모 노드 인덱스 번호 * 2 + 1  
```python

class Heap:
def __init__(self, data):
    self.heap_array = list()
    self.heap_array.append(None)
    self.heap_array.append(data)

def move_down(self, popped_idx):
    left_child_popped_idx = popped_idx * 2
    right_child_popped_idx = popped_idx * 2 + 1
    
    # case1: 왼쪽 자식 노드도 없을 때(자식이 없을 때)
    if left_child_popped_idx >= len(self.heap_array):
        return False
    # case2: 오른쪽 자식 노드만 없을 때(왼쪽 자식만 있을 때)
    elif right_child_popped_idx >= len(self.heap_array):
        if self.heap_array[popped_idx] < self.heap_array[left_child_popped_idx]: # 자식이 더 크면 True 반환
            return True # swap 실행
        else:
            return False # swap 중단
    # case3: 왼쪽, 오른쪽 자식 노드 모두 있을 때(자식이 2개 있을 때)
    else:
        if self.heap_array[left_child_popped_idx] > self.heap_array[right_child_popped_idx]: # 왼쪽 자식 > 오른쪽 자식일 때
            if self.heap_array[popped_idx] < self.heap_array[left_child_popped_idx]: # 부모 노드 < 왼쪽 자식이면
                return True 
            else:
                return False
        else: # 왼쪽 자식 < 오른쪽 자식일 때
            if self.heap_array[popped_idx] < self.heap_array[right_child_popped_idx]: # 부모 노드 < 오른쪽 자식이면
                return True
            else:
                return False

def pop(self):
    if len(self.heap_array) <= 1:
        return None
    
    returned_data = self.heap_array[1] # 가장 큰 값 리턴
    self.heap_array[1] = self.heap_array[-1] # 마지막 데이터를 리턴한 자리에 채움
    del self.heap_array[-1] # 마지막 데이터 삭제
    popped_idx = 1
    
    while self.move_down(popped_idx): # move_down 메서드가 True면 swap을 반복적으로 진행
        left_child_popped_idx = popped_idx * 2
        right_child_popped_idx = popped_idx * 2 + 1

        # case2: 오른쪽 자식 노드만 없을 때(왼쪽 자식만 있을 때)
        if right_child_popped_idx >= len(self.heap_array):
            if self.heap_array[popped_idx] < self.heap_array[left_child_popped_idx]: #왼쪽 자식과 부모를 바꾼다.
                self.heap_array[popped_idx], self.heap_array[left_child_popped_idx] = self.heap_array[left_child_popped_idx], self.heap_array[popped_idx]
                popped_idx = left_child_popped_idx
        # case3: 왼쪽, 오른쪽 자식 노드 모두 있을 때(2개의 자식이 있을 때)
        else:
            if self.heap_array[left_child_popped_idx] > self.heap_array[right_child_popped_idx]: # 왼쪽 자식 > 오른쪽 자식일 때
                if self.heap_array[popped_idx] < self.heap_array[left_child_popped_idx]: # 부모 노드 < 왼쪽 자식이 크다면, 부모와 왼쪽 자식 swap
                    self.heap_array[popped_idx], self.heap_array[left_child_popped_idx] = self.heap_array[left_child_popped_idx], self.heap_array[popped_idx]
                    popped_idx = left_child_popped_idx
            else: # 왼쪽 자식 < 오른쪽 자식일 때
                if self.heap_array[popped_idx] < self.heap_array[right_child_popped_idx]: # 부모 노드 < 오른쪽 자식이 크다면, 부모와 오른쪽 자식 swap
                    self.heap_array[popped_idx], self.heap_array[right_child_popped_idx] = self.heap_array[right_child_popped_idx], self.heap_array[popped_idx]
                    popped_idx = right_child_popped_idx
    
    return returned_data # 힙에서 가장 큰 값 리턴

def move_up(self, inserted_idx):
    if inserted_idx <= 1:
        return False
    parent_idx = inserted_idx // 2
    if self.heap_array[inserted_idx] > self.heap_array[parent_idx]: # 자식 노드 > 부모 노드이면
        return True # swap 실행
    else:
        return False # swap 중단

def insert(self, data):
    if len(self.heap_array) == 1:
        self.heap_array.append(data)
        return True
    
    self.heap_array.append(data) # data를 넣고,
    inserted_idx = len(self.heap_array) - 1 # 해당 data의 인덱스 번호를 저장(부모 노드와 비교하여 큰 값을 root 노드로 올리기 위해)
    
    # 부모 노드와 넣은 data 인덱스를 부모 노드 보다 작을 때 까지 반복 비교하며 부모 노드와 swap 반복
    while self.move_up(inserted_idx): 
        parent_idx = inserted_idx // 2
        self.heap_array[inserted_idx], self.heap_array[parent_idx] = self.heap_array[parent_idx], self.heap_array[inserted_idx]
        inserted_idx = parent_idx
    return True
```


## 힙의 시간 복잡도
---
힙에서 데이터 삽입 또는 삭제 시, 최악의 경우엔 root 노드 부터 leaf 노드 까지 비교를 해야한다. 이진 트리의 특성상 logN 번의 비교를 해야하므로, 힙의 시간 복잡도는`O(logN)`이라고 할 수 있다.  
힙에 데이터를 넣고, 최대값과 최소값을 찾으면 O(logN) 이 걸리기 때문에, 일반 배열에 비해 매우 효율적이라고 할 수 있다.