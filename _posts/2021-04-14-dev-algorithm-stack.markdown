---
layout: post
title:  "[자료구조] 스택(stack)"
subtitle:   "자료구조 스택 정리"
categories: dev
tags: algorithm stack
comments: false
---

## 개요
> 자료구조 `스택(stack)`에 대한 정리글입니다.

- 목차
	- [스택(stack)이란?](#스택stack이란) 
  - [스택 구현](#스택-구현)

## 스택(stack)이란?
---

* __스택(stack)__  
자료구조 중 하나로 가장 나중에 넣은 데이터를 가장 먼저 꺼내는 구조이다. 한쪽 끝에서만 자료를 넣거나 뺄 수 있는 자료구조이다.  
> 스택(stack)을 쉽게 말하자면, 패스트푸드점에 놓여있는 쟁반을 쌓고, 가져가는 것과 동일한 구조

* __스택의 구조__  
스택은 `LIFO(Last In, Fisrt Out)` 또는 `FILO(First In, Last Out)` 데이터 관리 방식을 따른다.
> LIFO: 마지막에 넣은 데이터를 가장 먼저 꺼낸다.  
> FILO: 먼저 넣은 데이터를 가장 마지막에 꺼낸다. 

  - __스택의 주요 기능__  
  push: 데이터를 스택에 넣기  
  pop: 데이터를 스택에서 꺼내기
![이미지1](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-14-dev-algorithm-stack-picture1.png)


## 스택 구현
---

* __스택 구현해보기__    
Python에서는 기존의 list가 곧 스택의 기능을 수행한다고 볼 수 있다. 따라서, Python의 list를 사용하여 스택을 구현할 수 있다.

```python
list_stack = list() #스택 자료구조 생성

list_stack.append(1) #스택에 데이터 삽입
list_stack.append(2)

list_stack.pop() #가장 나중에 넣은 데이터를 반환. 여기서는 2 반환.
```
