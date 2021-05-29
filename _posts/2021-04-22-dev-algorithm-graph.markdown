---
layout: post
title:  "[자료구조] 그래프(Graph)"
subtitle:   "자료구조 그래프 정리"
categories: dev
tags: algorithm graph
comments: false
---

## 개요
> 자료구조 `그래프(Graph)`에 대한 정리글입니다.

- 목차
    - [그래프(Graph)란?](#그래프graph란) 
    - [그래프 종류](#그래프-종류)
    - [그래프와 트리의 차이](#그래프와-트리의-차이)
    - [그래프 표현](#그래프-표현)

## 그래프(Graph)란?
---

* __그래프(Graph)__  
그래프는 `노드(Node)`와 그 노드를 연결하는 `간선(Edge)`을 하나로 모아놓은 자료구조이다. 실제 세계의 현상이나 사물을 노드와 간선으로 표현하기 위해 사용한다.
기본적으로 우리는 컴퓨터로 어떠한 작업을 하기 위해서는 실제 상황을 컴퓨터 내에 표현을 해야한다. 이러한 점에서 그래프는 실세계의 더욱 복잡한 상황이나 구조를 컴퓨터 내의 자료구조로 나타낼 수 있다.
> 실제 세계의 지도, 지하철 노선도 등은 그래프의 예시이다.

* __그래프 관련 용어__  
![이미지1](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-22-dev-algorithm-graph-picture1.png) 
 
  - 노드(Node), 정점(Vertex): 어떤 한 지점, 위치  
  - 간선(Edge): 위치 간의 관계를 표시한 선으로 노드를 연결한 선(link 또는 branch라고도 부름)  
  - 인접 정점(Adjacent Vertex): 간선으로 직접 연결된 정점  
  - 정점의 차수 (Degree): 무방향 그래프에서 하나의 정점에 인접한 정점의 수  
  - 진입 차수 (In-Degree): 방향 그래프에서 외부에서 오는 간선의 수  
  - 진출 차수 (Out-Degree): 방향 그래프에서 외부로 향하는 간선의 수  
  - 경로 길이 (Path Length): 경로를 구성하기 위해 사용된 간선의 수  
  - 단순 경로 (Simple Path): 처음 정점과 끝 정점을 제외하고 중복된 정점이 없는 경로  
    ('단순': 경로나 사이클에서 같은 정점을 두 번 이상 방문하지 않음을 의미)  
  - 사이클 (Cycle): 단순 경로의 시작 정점과 종료 정점이 동일한 경우  


## 그래프 종류
---
* __무방향 그래프(Undirected Graph)__  
간선에 방향이 표시되어 있지 않은 방향이 없는 그래프이다.  
정점 A, B 사이에 간선이 존재할 경우, A→B와 B→A를 의미한다. 간선을 통해 노드는 양방향으로 갈 수 있으므로 양방향 그래프라고도 불린다.  
보통 노드 A, B가 연결되어 있으면 (A, B) 또는 (B, A)로 표기한다.  

* __방향 그래프(Directed Graph)__  
간선에 방향이 있는 그래프이다.  
노드 A에서 B로 가는 간선으로 연결되어 있을 경우, < A, B >로 표기한다.  
보통 노드 A, B가 연결되어 있으면 (A, B) 또는 (B, A)로 표기한다.  

* __가중치 그래프(Weighted Graph) 또는 네트워크(Network)__  
간선에 비용 또는 가중치가 할당된 그래프이다.  

![이미지2](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-22-dev-algorithm-graph-picture2.png)  


* __연결 그래프(Connected Graph) 와 비연결 그래프(Disconnected Graph)__  
무방향 그래프에서 모든 노드에 대해 항상 경로가 존재하는 경우 연결 그래프, 특정 노드에 대해 경로가 존재하지 않는 경우는 비연결 그래프이다.  
> 비연결 그래프 예
> ![이미지3](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-22-dev-algorithm-graph-picture3.png)  

* __사이클(Cycle)과 비순환 그래프(Acyclic Graph)__  
단순 경로의 시작 노드와 종료 노드가 동일한 경우 사이클이라고 하며, 이 외의 경우를 사이클이 없는 그래프라고 한다.  
> 비순환 그래프 예
> ![이미지4](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-22-dev-algorithm-graph-picture4.png)  

* __완전 그래프(Complete Graph)__  
그래프의 모든 노드가 서로 연결되어 있는 그래프  
![이미지5](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-22-dev-algorithm-graph-picture5.png)  


## 그래프와 트리의 차이
---
트리는 그래프 중에 속한 특별한 종류라고 볼 수 있다.  
![이미지6](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-22-dev-algorithm-graph-picture6.png)  


## 그래프 표현
---
그래프의 정의와 종류에 대해서 알아보았다. 그렇다면, 이러한 그래프를 어떻게 코드로 구현할 수 있을까?  
그래프를 표현하는 방법에는 `인접 행렬`과 `인접 리스트` 방식이 있다.  

* __인접 행렬__  
2차원 배열로 그래프의 연결 관계를 표현하는 방식이다. 2차원 배열로 표현해야 하므로 메모리 소모가 크지만, 탐색 속도가 빠르다.
![이미지7](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-22-dev-algorithm-graph-picture7.png)  

* __인접 리스트__  
리스트로 그래프의 연결 관계를 표현하는 방식이다. 인접 행렬에 비해서 비교적 메모리 소모가 적지만, 탐색 속도가 인접 행렬에 비해 느리다고 할 수 있다.
![이미지8](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-22-dev-algorithm-graph-picture8.png)  


## 참고

<https://daebaq27.tistory.com/25>
<https://pangtrue.tistory.com/147>
<https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html>
<https://sarah950716.tistory.com/12>