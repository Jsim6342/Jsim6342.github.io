---
layout: post
title:  "[알고리즘] 정렬(Sort) 알고리즘"
subtitle:   "정렬 알고리즘 정리"
categories: dev
tags: Sort
comments: false
---

## 개요
> `정렬 알고리즘`에 대한 정리글입니다.

- 목차
	- [정렬(Sort)이란?](#정렬(Sort)이란?) 
    - [버블 정렬(bubble sort)](#버블-정렬(bubble-sort))
    - [삽입 정렬(insertion sort)](#삽입-정렬(insertion-sort))
    - [선택 정렬(selection sort)](#선택-정렬(selection-sort))
    - [병합 정렬(merge sort)](#병합-정렬(merge-sort))
    - [퀵 정렬(quick sort)](#퀵-정렬(quick-sort))


## 정렬(Sort)이란?
---

* __정렬(Sort)__  
정렬(sorting)은 어떤 데이터들이 주어졌을 때 이를 `순서대로 나열`하는 것이다.
> 컴퓨터 과학과 수학에서 정렬 알고리즘은 원소들을 번호순이나 사전 순서와 같이 일정한 순서대로 열거하는 알고리즘이다. 효율적인 정렬은 탐색이나 병합 알고리즘처럼 다른 알고리즘을 최적화 하는 데 매우 중요하다. 또, 정렬 알고리즘은 데이터의 정규화나 의미있는 결과물을 생성하는 데 흔히 유용하게 쓰인다. _출처: 위키백과

* __정렬 알고리즘__  
정렬 기능을 수행하는 다양한 알고리즘이 고안되었다. 그 중에서도 `버블 정렬`, `선택 정렬`, `삽입 정렬` 부터 고급 정렬인 `병합 정렬`과 `퀵 정렬`에 대해 정리해보았다.


## 버블 정렬(bubble sort)
---

* __버블 정렬이란?__  
두 인접한 데이터를 비교해서, 앞에 있는 데이터가 뒤에 있는 데이터 보다 클 경우, 자리를 바꾸는 과정을 반복하는 알고리즘. 버블 정렬은 다음과 같은 과정으로 진행된다.
> 1. 인접한 두 원소를 비교한다. (배열의 맨 앞 원소 부터 비교 시작)  
> 2. 앞의 원소가 뒤의 원소 보다 크다면, 자리를 바꾼다.  
> 3. 1,2 번 과정을 반복하면, 맨 뒤에 큰 수가 위치하게 된다.  
> 4. 다시 처음 원소 부터 1,2번의 과정을 반복한다.  

* __버블 정렬 구현__  
현 원소와 그 다음 원소를 비교하는 작업을 반복하므로, 데이터 길이 - 1번을 반복한다.  
인접한 두 데이터를 비교해서 진행되는 1번의 패스스루 마다 가장 큰 값이 비교한 원소의 맨 뒤에 위치하게 된다.  따라서, 1번의 패스스루가 진행될 때 마다 마지막 원소 1개씩은 비교하지 않아도 된다.  
또한, 패스스루를 거쳤는데도 자리 바꿈이 1번도 일어나지 않았다면, 이미 정렬된 상태를 의미하므로 곧바로 리스트를 리턴해주면 된다.  
```python
def bubblesort(data_list):
	for i in range(len(data_list)-1): # 데이터 길이-1 만큼 반복
		swap = False # 자리 바꿈이 일어났는지 확인하기 위한 변수
		for j in range(len(data_list)-i-1): # 1회 패스스루 마다 1번씩 줄여 반복
			if data_list[j] > data_list[j+1]: # 이전 데이터가 이후 데이터 보다 크다면
				data_list[j], data_list[j+1] = data_list[j+1], data_list[j] # swap
				swap = True # 자리 바꿈이 일어났으므로, True로 변환
		if swap == False: # 자리 바꿈이 안일어났다면, 곧바로 break
			break
	return data_list
```  

* __버블 정렬의 시간복잡도__  
반복문이 2개인 형태로 O($$n^2$$)으로 볼 수 있다.  
최악의 경우 <font>$$\frac { n * (n - 1)}{ 2 }$$</font>  
완전 정렬이 되어 있는 상태로 최선의 경우 O(n)  


## 삽입 정렬(insertion sort)
---

* __삽입 정렬이란?__  
배열의 모든 요소를 앞에서부터 차례대로 이미 정렬된 배열 부분(key 값 보다 앞 부분)과 비교하여, 자신의 위치를 찾아 삽입함으로써 정렬을 완성하는 알고리즘이다. 삽입 정렬은 다음과같은 과정으로 진행된다.
> 1. Key 값을 정한다. (맨 처음 Key 값은 2번째 자리이다.)  
> 2. Key 값 보다 앞에 있는 값들(정렬된 값들)과 Key 값을 비교하여 Key 값을 올바른 위치에 놓는다.  
> 3. Key 값의 인덱스 값을 하나씩 늘려가며 1,2번의 과정을 반복한다.  

* __삽입 정렬 구현__  
패스스루 마다 Key는 반복 시작 위치+1이므로 데이터 길이-1 만큼 반복을 진행한다.  
첫 번째 반복문에서 key 값의 위치를 나타내는 i를 배정하고, i+1을 Key 값으로 하여 0번 인덱스 까지 Key 값 보다 큰 값을 Key 값의 뒤로 옮기는 자리 바꿈 작업을 반복하여 진행한다. Key 값의 위치가 올바른 위치에 있다면 곧바로 break를 하여 다음 Key 값을 비교하도록 한다.   
```python
def insertion_sort(data_list):
	for i in range(len(data_list)-1): # 데이터 길이-1 만큼 반복
		for j in range(i+1,0,-1): # i+1번째를 Key 값으로 하여 0번 인덱스까지 반복
			if data_list[j] < data_list[j-1]: # Key 값 보다 큰 값을 만나면 자리 바꿈을 통해 뒤로 보낸다.
				data_list[j], data_list[j-1] = data_list[j-1], data_list[j]
			else:
				break
	return data_list
```  

* __삽입 정렬의 시간복잡도__  
반복문이 2개인 형태로 O($$n^2$$)으로 볼 수 있다.  
최악의 경우 <font>$$\frac { n * (n - 1)}{ 2 }$$</font>  
완전 정렬이 되어 있는 상태로 최선의 경우 O(n)  


## 선택 정렬(selection sort)
---

* __선택 정렬이란?__  
배열을 탐색하며, 배열의 최소값을 가장 맨 앞으로 위치시키는 작업을 반복하여 정렬을 하는 알고리즘으로 다음과 같은 과정으로 진행된다.
> 1. 주어진 데이터 중, 최소값을 찾는다.  
> 2. 해당 최소값을 데이터 맨 앞에 위치한 값과 교환한다.  
> 3. 맨 앞의 위치를 뺀 나머지 데이터를 동일한 방법으로 반복해서 정렬한다.  

* __선택 정렬 구현__  
첫 번째 반복문에서 현재 시작 위치를 지정.  
두 번째 반복문을 돌며 가장 작은 값의 인덱스를 lowest 변수에 저장해두고, 시작 위치와 lowest 위치를 바꾼다.  
```python
def selection_sort(data_list):
	for start in range(len(data_list)-1):
		lowest = start # 최소값을 저장할 lowest 변수 선언
		for i in range(start+1,len(data_list)): # 시작 위치 이후의 최소값을 찾는다.
			if data_list[lowest] > data_list[i]:
				lowest = i
		data_list[lowest], data_list[start] = data_list[start], data_list[lowest] # 최소값과 자리 바꿈
	return data_list
```  

* __선택 정렬의 시간복잡도__  
반복문이 2개인 형태로 O($$n^2$$)으로 볼 수 있다.  
상세하게 계산하면, <font>$$\frac { n * (n - 1)}{ 2 }$$</font>  


## 병합 정렬(merge sort)
---

* __분할 정복__  
병합 정렬을 이해하기 위해서는 `분할 정복` 알고리즘에 대해 알아야한다. 
> 분할 정복: 문제를 나눌 수 없을 때까지 나누어서 각각을 풀면서 다시 합병하여 문제의 답을 얻는 알고리즘이다.  
> 하향식 접근법으로 상위의 해답을 구하기 위해 아래로 내려가면서 하위의 해답을 구하는 방식이다.  
> 분할 정복은 일반적으로 재귀함수로 구현한다.  
> 잘게 나눈 부분 문제들은 서로 중복되지 않는다.  

* __병합 정렬이란?__  
병합 정렬은 분할 정복의 알고리즘으로 배열 하나를 잘게 나눈 다음에 정렬을 시키고, 그것들을 다시 합쳐서 배열을 만드는 과정을 통해 정렬시키는 방법이다. 병합 정렬은 다음과 같은 과정으로 진행된다.
> 1. 분할 작업: 정렬하려는 배열을 가장 작은 단위까지 분할한다.  
> 2. 병합 작업: 분할한 배열 조각들을 정렬시키면서 서로 병합한다.  

* __병합 정렬 구현__  
mergesplit 함수에서 중간값을 기준으로 left와 right로 배열을 나누는 작업을 길이가 1이 될 때까지 한다.  
나누어진 배열에서 작은 값을 먼저 꺼내어 붙이며 합치는 merge 함수를 반복적으로 수행하여 병합 정렬을 수행한다.  

```python
def mergesplit(data_list):
	if len(data_list) <= 1: # 길이가 1이하면 반환
		return data_list
	mid = int(len(data) / 2) # 중간 값을 기준으로 나눈다.
	# 나눈 배열을 다시 mergesplit 함수에 넣는다 (재귀)
	left = mergesplit(data_list[:mid]) 
	right = mergesplit(data_list[mid:])
	return merge(left, right)

def merge(left, right):
	merged = list()
	lp, rp = 0, 0
	
	# left, right 둘 다 남아 있을 때
	while len(left) > lp and len(right) > rp:
		if left[lp] > right[rp]:
			merged.append(right[rp])
			rp += 1
		else:
			merged.append(left[lp])
			lp += 1

	# left만 남아 있을 때
	while len(left) > lp:
		merged.append(left[lp])
		lp += 1
	# right만 남아 있을 때
	while len(right) > rp:
		merged.append(right[rp])
		rp += 1

	return merged
```  

* __병합 정렬의 시간 복잡도__  
분할 단계에서 분할이 되는 깊이는 2의 제곱으로 증가하므로 데이터의 갯수가 n일 때, 깊이는 $$log_2 n$$이라 할 수 있다.  
병합 단계에서 단계(깊이) 마다 노드의 갯수는 $$2^i$$개, 리스트의 길이는 n/$$2^i$$개이다. 노드 마다 리스트의 길이 만큼 비교가 진행되므로 각 단계에서의 시간 복잡도는 <font>$$2^i * \frac { n }{ 2^i } = O(n)$$</font>이다.  
각 단계 마다 O(n)의 시간 복잡도에 단계가 총 $$log_2 n$$이므로 O(n) * O(log n) = O(n log n)이라 할 수 있다.  


## 퀵 정렬(quick sort)
---

* __퀵 정렬이란?__  
기준 원소(피벗, pivot)를 하나 정하고, 피벗을 기준으로 배열의 원소들을 비교하여, 작은 원소들은 피벗 앞에, 큰 원소들은 피벗 뒤에 배치한다. 피벗을 기준으로 왼쪽, 오른쪽으로 배치된 배열을 또 다시 같은 방법으로 반복 배치를 수행하고, 이를 다시 합하여 정렬하는 방법이다.
퀵 정렬은 다음과 같은 순서로 수행된다.
> 1. 분할: 피벗을 기준으로 2개의 부분 배열로 나눈다. 이를 배열의 크기가 충분히 작아질 때까지 수행한다.  
> 2. 결합: 피벗으로 분할된 배열들을 다시 합한다.  

* __퀵 정렬 구현__  
```python
def qsort(data_list):
	if len(data) <= 1:
		return data_list
	
	left, right = list(), list() # 피벗을 기준으로 나눌 리스트 선언
	pivot = data_list[0] # 배열의 첫 번째 원소를 피벗으로 한다.
	
	# 피벗을 기준으로 작은 원소는 왼쪽, 큰 원소는 오른쪽 배열에 삽입
	for i in range(1, len(data_list)):
		if pivot > data_list[i]:
			left.append(data_list[i])
		else:
			right.append(data_list[i])
	
	# list comprehension 활용한 코드 선언
	# left = [item for item in data[1:] if pivot > item]
    # right = [item for item in data[1:] if pivot < item]

	return qsort(left) + [pivot] + qsort(right) # 만들어진 배열을 qsort()함수로 재귀적 수행, pivot과 결합
```  

* __퀵 정렬의 시간복잡도__  
병합 정렬과 유사한 원리로 O(n logn)의 시간 복잡도를 갖는다.
단, pivot 보다 모든 원소가 크거나, 작을 경우 모든 데이터를 비교하는 상황이 나오게 된다. 이 경우에는 단계가 n번 비교가 n번 일어나므로 O($$n^2$$)의 시간 복잡도를 갖는다.