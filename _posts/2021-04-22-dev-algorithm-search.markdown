---
layout: post
title:  "[알고리즘] 순차 탐색, 이진 탐색"
subtitle:    알고리즘 순차 탐색, 이진 탐색 정리"
categories: dev
tags: algorithm search
comments: false
---

## 개요
>  `순차 탐색`, `이진 탐색` 알고리즘에 대한 정리글입니다.

- 목차
    - [순차 탐색(Sequential Search)](#순차-탐색sequential-search)
    - [이진 탐색(Binary Search)](#이진-탐색binary-search)
	- [이진 탐색 활용](#이진-탐색-활용)


## 순차 탐색(Sequential Search)
---

* __순차 탐색(Sequential Search)이란?__  
순차 탐색은 말 그대로 배열에서 원하는 데이터를 찾아낼 때, 처음부터 끝까지 `차례대로 비교`하며 원하는 데이터를 찾아내는 알고리즘으로 단방향으로 탐색을 하기 때문에 `선형 탐색(Linear Search)`이라고 부르기도 한다.  
순차 탐색은 데이터를 따로 조작할 필요가 없어 매우 단순하지만, 다른 알고리즘에 비해 비효율적이라는 단점을 지니고 있다.  

* __순차 탐색 구현__  
순차 탐색은 찾고자하는 데이터와 데이터가 담겨있는 리스트를 앞에서부터 하나씩 비교하는 방식으로 구현하면된다.  
```python
def sequential(data_list, data):
	for i in range(len(data_list)):
		if data_list[i] == data:
			return i
	return -1
```  

* __순차 탐색의 시간 복잡도__  
평균적인 시간 복잡도는 n개의 데이터가 있을 때, 찾고자 하는 데이터가 있는 경우와 없는 경우로 나누어 생각해볼 수 있다. 데이터가 없는 경우는 n번 탐색해야하고, 있는 경우는 평균 n/2번을 탐색해야 하므로, 이 경우를 모두 종합하여 계산하면 다음과 같다.  
![이미지1](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-22-dev-algorithm-search-picture1.png)  
최악의 경우는 리스트를 모두 탐색하는 n번이다. 빅 오 표기법은 최악의 경우를 기준으로하며, 순차 탐색의 평균 시간 복잡도는 상황에 따라 변동 가능성이 높아 최악의 경우를 택하는 것이 적절하다고 볼 수 있다. 따라서, 순차 탐색의 `시간 복잡도는 O(n)`이라 볼 수 있다.  


## 이진 탐색(Binary Search)
---

* __이진 탐색(Binary Search)이란?__  
이진 탐색은 탐색할 자료를 둘로 나누어 해당 데이터가 있을만한 범위를 지정하고, 또 다시 둘로 나누어 탐색하는 과정을 반복하여 범위를 줄여나가면서 데이터를 탐색하는 방법이다.  
이진 탐색은 찾고자하는 데이터와 크고, 작음을 비교하며 범위를 줄여나가야 하기 때문에 선 정렬 조건이 필요하다.  
> CS50 강의에서 데이비드 말런 교수님은 약 1000페이지가 되는 전화번호부에서 원하는 번호를 찾는 예시를 들어주었다. 순차 탐색으로 한다면 1페이지 부터 차근차근 탐색하면 된다.  
> 하지만, 이진 탐색으로 한다면, 1000/2인 500페이지를 먼저 열고, 필요 없는 절반을 버린다. 다시, 500/2인 250페이지를 열고 필요 없는 부분을 버린다. 이 과정을 반복하며 범위를 절반씩 줄여나가서 결국 원하는 데이터를 찾는 방법이다. 데이터가 절반씩 줄어나간다는 점에서 일반적인 순차 탐색 보다 매우 효율적인 탐색 방법이 될 수 있다고 볼 수 있다.  

* __이진 탐색 활용__  
이진 탐색을 활용하는 방법에는 `재귀 함수`를 이용한 방법과 `반복문`을 이용하는 방법이 있다. 아래의 이진 탐색 코드는 이진 탐색과 관련된 알고리즘 문제를 풀 때 기본 바탕이 되는 코드라고 볼 수 있다. 해당 코드는 '이것이 코딩 테스트다_나동빈' 책을 참고하였다.

  - __재귀 함수로 구현__

```python
def binary_search(array, target, start, end):

	if start > end:
		return None

	mid = (start + end) // 2

	if array[mid] == target:
		return mid
	elif array[mid] > target:
		return binary_search(array, target, start, mid - 1)
	else:
		return binary_search(array, target, mid + 1, end)

n, target = list(map(int, input().split()))
array = list(map(int, input().split()))

result = binary_search(array, target, 0, n - 1)
if result == None:
	pirnt("원소가 존재하지 않습니다.")
else:
	print(result + 1)
```  

  - __반복문으로 구현__

```python
def binary_search(array, target, start, end):

	while start <= end:
		mid = (start + end) // 2
		if array[mid] == target:
			return mid
		elif array[mid] > target:
			end = mid - 1
		else:
			start = mid + 1

	return None

n, target = list(map(int, input().split()))
array = list(map(int, input().split()))

result = binary_search(array, target, 0, n - 1)
if result == None:
	pirnt("원소가 존재하지 않습니다.")
else:
	print(result + 1)
```  

* __이진 탐색의 시간 복잡도__  
탐색을 1회 수행하면, 범위가 절반씩 줄어든다. 따라서 1/2의 제곱 단위로 탐색 연산이 줄어들며 수행되므로 logn으로 볼 수 있다. 즉, 이진 탐색의 시간 복잡도는 `O(logn)`으로 볼 수 있다.


## 이진 탐색 활용
---
* __파라메트릭 서치__  
파라메트릭 서치는 이진 탐색 알고리즘을 이용하여 최대, 최소값을 구하는 방법이다. 보통 이진 탐색을 활용한 알고리즘 문제는 파라메트릭 서치를 기반으로 한 문제가 많다.  
기본적으로 찾고자하는 값을 이진 탐색으로 구하려고 할 때, 범위에 따라 조건을 수행해보고, 조건을 만족하면 답으로 출력할 것이다. 하지만, 조건을 만족하는 값이 여러 개라면 어떨까?  
보통 이런 문제는 최대, 최소값을 문제에서 요구한다. 이 때 이진 탐색을 활용하여 조건을 만족하는 최대, 최소값을 구할 수 있다.  
최소, 최대값(최소, 최대 index 위치)을 찾고 싶을 때는 target과 array[mid]의 조건에서 '='(등호)의 조건을 통해 구현한다.  
이진 탐색을 구현하는 조건 코드에서 등호가 들어간 조건에 따라 최소, 최대값을 구할 수 있다. 즉, array(mid) ≥ target라면,  mid값이 조건을 만족하는 가장 왼쪽 값에 가까워질 것이고, array(mid) ≤ target라면, mid값이 조건을 만족하는 가장 오른쪽 값에 가까워질 것이다.  


## 참고

책: '이것이 코딩 테스트다'_나동빈