---
layout: post
title:  "[자료구조] 큐(queue)"
subtitle:   "자료구조 큐 정리"
categories: dev
tags: queue
comments: false
---

## 개요
> 자료구조 `큐(queue)`에 대한 정리글입니다.

- 목차
	- [큐(queue)란?](#큐(queue)란?) 
	- [큐 종류](#큐-종류?)
    - [큐 구현](#큐-구현)

## 큐(queue)란?
---

*__큐(queue)__  
자료구조 중 하나로 가장 먼저 넣은 데이터를 가장 먼저 꺼내는 구조.  
> 큐(queue)를 쉽게 말하자면, '선입선출', 줄을 서는 것  
![이미지1](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-14-dev-algorithm-queue-picture1.png)

*__큐의 구조__  
큐의 구조는 가장 먼저 넣은 데이터를 가장 먼저 꺼낼 수 있는 구조로 되어 있다.
  - __큐의 주요 기능__  
  Enqueue: 큐에 데이터를 넣는 기능  
  Dequeue: 큐에서 데이터를 꺼내는 기능
![이미지2](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-14-dev-algorithm-queue-picture2.png)



## 큐 종류?
---

*__큐의 종류__  
큐에는 다양한 종류들이 있다. Python의 queue 라이브러리는 이러한 다양한 큐들을 제공해주고 있다. 각 큐에 대해 간단하게 알아보자.
  - __Queue__  
  가장 일반적인 큐 자료구조로 `FIFO(First-In,First-Out)`방식으로 진행된다.
```python
import queue

list_queue = queue.Queue() #큐 자료구조 생성
list_queue.put(1) #큐에 데이터 삽입
list_queue.put(2) 
list_queue.get() #큐에서 데이터를 꺼내고 반환. 여기서는 1 반환.
```
  - __LifoQueue__  
  나중에 입력된 데이터가 먼저 출력되는 구조로 `LIFO(Last-In,First-Out)`방식으로 진행된다.(스택 구조)
```python
import queue

list_queue = queue.LifoQueue() #큐 자료구조 생성
list_queue.put(1) #큐에 데이터 삽입
list_queue.put(2) 
list_queue.get() #큐에서 데이터를 꺼내고 반환. 여기서는 2 반환.
```
  - __PriorityQueue__  
  데이터마다 우선순위가 있고, 우선순위가 높은 순서로 데이터가 출력되는 형식이다. 여기서 우선순위가 높다는 것은 우선순위의 값이 작다는 것을 의미한다.
```python
import queue

list_queue = queue.PriorityQueue() #큐 자료구조 생성
list_queue.put((1, "one")) #큐에 데이터 삽입. 우선순위도 함께 넣어줘야 한다.
list_queue.put((2, "two")) 
list_queue.get() #큐에서 데이터를 꺼내고 반환. 여기서는 one 반환.
```


## 큐 구현
---

*__큐 구현해보기__  
python에서는 기존의 list를 활용하여 간단하게 큐를 구현해 볼 수 있다.
```Python
list_queue = list()

def enqueue(data):
    list_queue.append(data)

def dequeue():
    data = list_queue[0]
    del list_queue[0]
    return data
```
