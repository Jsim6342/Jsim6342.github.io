---
layout: post
title:  "[자료구조] 연결 리스트(linked list)"
subtitle:   "자료구조 연결 리스트 정리"
categories: dev
tags: algorithm linked list
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

* __C언어로 연결 리스트 구현__  
다음 코드는 C언어로 연결 리스트를 구현한 코드이다.  
해당 코드는 CS50 수업을 참고하여 작성하였다.  
파이썬 코드가 궁금하다면, [잔재미코딩](https://www.fun-coding.org/Chapter07-linkedlist-live.html)을 참고하기 바란다.

```c
#include <stdio.h>
#include <stdlib.h>

//연결 리스트의 기본 단위가 되는 node 구조체를 정의합니다.
typedef struct node
{
    int number; 
    struct node *next;
}
node;

int main(void)
{

    node *list = NULL;

    node *n = malloc(sizeof(node));
    if (n == NULL)
    {
        return 1;
    }

    n->number = 1;
    n->next = NULL;
    list = n;

    n = malloc(sizeof(node));
    if (n == NULL)
    {
        return 1;
    }

    n->number = 2;
    n->next = NULL;
    list->next = n;

    n = malloc(sizeof(node));
    if (n == NULL)
    {
        return 1;
    }

    n->number = 3;
    n->next = NULL;


    list->next->next = n;

    for (node *tmp = list; tmp != NULL; tmp = tmp->next)
    {
        printf("%i\n", tmp->number);
    }

    while (list != NULL)
    {
        node *tmp = list->next;
        free(list);
        list = tmp;
    }
}
```