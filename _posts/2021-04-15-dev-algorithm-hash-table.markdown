---
layout: post
title:  "[자료구조] 해시 테이블(hash table)"
subtitle:   "자료구조 해시 테이블 정리"
categories: dev
tags: algorithm hash table
comments: false
---

## 개요
> 자료구조 `해시 테이블(hash table)`에 대한 정리글입니다.

- 목차
	- [해시 테이블(hash table)이란?](#해시-테이블hash-table이란) 
    - [해시 테이블 장단점](#해시-테이블-장단점)
    - [해시 테이블 구현](#해시-테이블-구현)
    - [해시 테이블 충돌](#해시-테이블-충돌)
    - [시간 복잡도](#시간-복잡도)

## 해시 테이블(hash table)이란?
---

* __해시 테이블(hash table)__  
`키(Key)`와 `데이터(Value)`로 이루어진 구조로 키에 대한 데이터를 저장하는 자료 구조이다. 각각의 키 마다 데이터를 저장하고 있으므로 저장 공간은 많이 필요할 수 있지만 데이터를 찾는 속도는 매우 빠르게 유지할 수 있다.
> 파이썬에서 기본적으로 제공하고 있는 `딕셔너리(Dictionary)`는 해시 테이블의 예라고 할 수 있다.
해시 테이블은 저장, 삭제, 읽기가 빈번하고 검색이 많이 필요한 경우 사용된다.
> 중복 확인이 쉽기 때문에 캐쉬 구현시 사용된다.

* __해시 테이블 용어__  
  - 키(Key): 해싱 함수를 거치기 전 데이터. 본래 데이터 값
  - 해시 테이블(Hash Table): 키 값의 연산에 의해 직접 접근이 가능한 데이터 구조
  - 해싱 함수(Hashing Function): 효율적인 데이터 관리를 위해 임의의 데이터를 고정된 길이의 데이터로 변환해주는 함수
  - 해시 값(Hash Value), 해시(Hash) or 해시 주소(Hash Address): 해싱 함수로 연산되어 나온 데이터 값
  - 슬롯(Slot): 한 개의 데이터를 저장할 수 있는 공간(저장소)


## 해시 테이블 장단점
---
배열과 비교하여 해시 테이블에는 장단점이 있다. 상황에 따라 적절한 자료구조를 사용하면 된다.  

* __장점__  
  - 데이터 저장과 읽기 속도가 매우 빠르다.
  - 키 마다 데이터가 있으므로 중복 확인이 쉽다.

* __단점__  
  - 키 마다 데이터를 저장해야 하므로 저장공간이 많이 필요하다.
  - 해싱 함수로 배정된 해시 주소가 동일할 경우 충돌이 일어난다. (충돌 해결을 위한 대안이 필요)


## 해시 테이블 구현
---

* __해시 함수__  
대표적으로 잘 알려진 안전한 해시 함수에는 `SHA` 함수가 있다.

```python
import hashlib

# SHA-1
data = 'test'.encode() # string형태는 byte형태로 인코딩 해줘야 해시함수를 추출할 수 있다.
hash_object = hashlib.sha1() # hashlib에 있는 sha1을 사용
hash_object.update(data) # 인코딩한 데이터를 넣어서 해시 값으로 만들어준다.
hex_dig = hash_object.hexdigest() # hexdigest(): 16진수로 추출

# SHA-256
data = 'test'.encode()
hash_object = hashlib.sha256()
hash_object.update(data)
hex_dig = hash_object.hexdigest()
```

* __해시 테이블 구현__  
파이썬으로 간단하게 해시 테이블을 구현해보자. 크기가 8인 해시 테이블을 만들어 보았다. 여기서 hash_function() 뿐만 아니라 get_key()함수를 만들어 데이터를 hash()함수로 가공시키는 작업을 따로 빼주었다. 이는 차후에 hash()함수 외에 SHA-256 함수로 대체할 수 있으며, 해시 테이블의 충돌 방지에 key와 value를 저장하는 작업에서 암호화된 key를 변수로 저장할 수 있다. (hash_function만 만들게 되면, 기존의 data와 해시 값만 있기 때문에 암호화된 정보로 key를 저장할 수 없다.)

```python
#해시테이블 공간 생성
hash_table = list([0 for i in range(8)])

#key 생성
#hash() 함수를 사용하여 고유값 반환
def get_key(data):
    return hash(data)

#해시 함수
#나눗셈 법을 이용한 해시 함수를 만들어 고정된 길이 반환
def hash_function(key):
    return key % 8

#해시 함수 저장
def save_data(data, value):
    hash_address = hash_function(get_key(data))
    hash_table[hash_address] = value
    
#해시 함수 출력
def read_data(data):
    hash_address = hash_function(get_key(data))
    return hash_table[hash_address]
```

## 해시 테이블 충돌
---
동일한 해시 주소에 다른 데이터가 저장될 경우 충돌이 발생하게 된다. 해시 테이블의 충돌을 방지하는 방법으로 다음과 같이 대표적인 2가지 방법이 있다.

* __Chaining기법__  
해시 테이블 저장공간 외의 공간을 활용하는 기법이다. 충돌이 일어나면, `링크드 리스트를 활용`하여 링크드 리스트로 데이터를 추가로 뒤에 연결시키는 방법이다.
충돌을 해결했지만, 링크드 리스트를 추가한 만큼 저장공간이 소모되며, 링크드 리스트를 탐색하는 동안엔 검색 속도가 해시 테이블에 비해 낮아진다.

```python
import hashlib

hash_table = list([0 for _ in range(8)])

def get_key(data):
    hash_object = hashlib.sha256()
    hash_object.update(data.encode())
    hex_dig = hash_object.hexdigest()
    return int(hex_dig, 16) #!6진수 문자열을 int형으로 변환

def hash_function(key):
    return key % 8

def save_data(data, value):
    key = get_key(data)
    hash_address = hash_function(key)
    if hash_table[hash_address] != 0: # 충돌 발생 시,
        for index in range(len(hash_table[hash_address])): # 해당 해시 주소의 리스트 길이만큼 탐색
            if hash_table[hash_address][index][0] == key: # 일치하는 키가 있다면
                hash_table[hash_address][index][1] = value # 그곳의 데이터를 업데이트
                return
        hash_table[hash_address].append([key,value]) # 일치하는 데이터가 없다면, 새로 추가
    else:
        hash_table[hash_address] = [[key, value]] # 충돌 발생이 없을 경우, 데이터 추가

def read_data(data):
    key = get_key(data)
    hash_address = hash_function(key)
    if hash_table[hash_address] != 0:
        for index in range(len(hash_table[hash_address])):
            if hash_table[hash_address][index][0] == key:
                return hash_table[hash_address][index][1]
        return None
    else:
        return None
```

* __Linear Probing기법__  
해시 테이블 저장공간 안에서 충돌 문제를 해결하는 기법이다. 충돌이 일어나면, 충돌이 일어난 주소의 다음 주소를 탐색하기 시작하여 맨 처음으로 나오는 빈 공간에 데이터를 저장한다.
추가적인 저장 공간 소모 없이 충돌을 해결했지만, 특정 주소값에 충돌이 몰릴 경우 저장과 검색의 효율성이 떨어질 수 있다.

```python
import hashlib

hash_table = list([0 for i in range(8)])

def get_key(data):
    hash_object = hashlib.sha256()
    hash_object.update(data.encode())
    hex_dig = hash_object.hexdigest()
    return int(hex_dig, 16)

def hash_function(key):
    return key % 8

def save_data(data, value):
    index_key = get_key(data)
    hash_address = hash_function(index_key)

    if hash_table[hash_address] != 0: # 충돌이 발생하면,
        for index in range(hash_address, len(hash_table)): # 충돌 지점 부터 탐색
            if hash_table[index] == 0: # 빈 저장소를 발견하면,
                hash_table[index] = [index_key, value]
                return
            elif hash_table[index][0] == index_key: #이미 key값 데이터가 있어 업데이트하는 경우
                hash_table[index][1] = value
                return
    else: # 충돌이 발생하지 않는 경우,
        hash_table[hash_address] = [index_key, value]

def read_data(data):
    index_key = get_key(data)
    hash_address = hash_function(index_key)
    
    if hash_table[hash_address] != 0:
        for index in range(hash_address, len(hash_table)):
            if hash_table[index] == 0:
                return None
            elif hash_table[index][0] == index_key:
                return hash_table[index][1]
    else:
        return None
```

## 시간 복잡도
---
  - 해시 테이블에서 검색을 하는 경우 시간 복잡도는 어떻게 될까? 일반적인 경우는 Key값으로 데이터를 바로 조회할 수 있으므로 `O(1)`의 시간 복잡도를 갖는다.
  - 하지만, 충돌이 모두 발생하는 최악의 경우에는 데이터의 갯수만큼 조회해야하기 때문에 O(n)의 시간 복잡도를 갖는다.
 