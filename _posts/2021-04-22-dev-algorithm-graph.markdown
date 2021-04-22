---
layout: post
title:  "[자료구조] 그래프(Graph)"
subtitle:   "자료구조 그래프 정리"
categories: dev
tags: Graph
comments: false
---

## 개요
> 자료구조 `그래프(Graph)`에 대한 정리글입니다.

- 목차
	- [그래프(Graph)란?](#그래프(Graph)란?) 
    - [그래프 종류](#그래프-종류)
    - [그래프와 트리의 차이](#그래프와-트리의-차이)
    - [그래프 알고리즘](#그래프-알고리즘)

## 그래프(Graph)란?
---

* __그래프(Graph)__  
그래프는 `노드(Node)`와 그 노드를 연결하는 `간선(Edge)`을 하나로 모아놓은 자료구조이다. 실제 세계의 현상이나 사물을 노드와 간선으로 표현하기 위해 사용한다.
> 실제 세계의 지도, 지하철 노선도 등은 그래프의 예시이다.

* __그래프 관련 용어__  
![이미지1](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-18-dev-algorithm-graph-picture1.png) 
 
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
![이미지2](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-18-dev-algorithm-graph-picture2.png)  

* __방향 그래프(Directed Graph)__  
간선에 방향이 있는 그래프이다.  
노드 A에서 B로 가는 간선으로 연결되어 있을 경우, < A, B >로 표기한다.  
보통 노드 A, B가 연결되어 있으면 (A, B) 또는 (B, A)로 표기한다.  
![이미지3](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-18-dev-algorithm-graph-picture3.png)  

* __가중치 그래프(Weighted Graph) 또는 네트워크(Network)__  
간선에 비용 또는 가중치가 할당된 그래프이다.  
![이미지4](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-18-dev-algorithm-graph-picture4.png)  

* __연결 그래프(Connected Graph) 와 비연결 그래프(Disconnected Graph)__  
무방향 그래프에서 모든 노드에 대해 항상 경로가 존재하는 경우 연결 그래프, 특정 노드에 대해 경로가 존재하지 않는 경우는 비연결 그래프이다.  
> 비연결 그래프 예
> ![이미지5](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-18-dev-algorithm-graph-picture5.png)  

* __사이클(Cycle)과 비순환 그래프(Acyclic Graph)__  
단순 경로의 시작 노드와 종료 노드가 동일한 경우 사이클이라고 하며, 이 외의 경우를 사이클이 없는 그래프라고 한다.  
> 비순환 그래프 예
> ![이미지6](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-18-dev-algorithm-graph-picture6.png)  

* __완전 그래프(Complete Graph)__  
그래프의 모든 노드가 서로 연결되어 있는 그래프  
![이미지7](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-18-dev-algorithm-graph-picture7.png)  


## 그래프와 트리의 차이
---
트리는 그래프 중에 속한 특별한 종류라고 볼 수 있다.  
![이미지8](https://jsim6342.github.io/assets/img/dev/algorithm/2021-04-18-dev-algorithm-graph-picture8.png)  
출처: https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html


## 그래프 알고리즘
---
실생활에서 다양한 것들을 그래프로 표현할 수 있는 만큼 그래프를 활용한 다양한 알고리즘이 고안되어 있다. 차후에 각 알고리즘 정리, 업로드를 통해 자세하게 설명하고자 한다.