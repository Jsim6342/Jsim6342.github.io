---
layout: post
title:  "[알고리즘] 최소 신장 트리(MST, Minimum Spanning Tree)"
subtitle:   "최소 신장 트리 알고리즘 정리"
categories: dev
tags: mst
use_math: False
comments: False
---

## 개요
> `최소 신장 트리(MST, Minimum Spanning Tree)`에 대한 정리글입니다.

- 목차
	- [최소 신장 트리](#최소-신장-트리) 
    - [크루스칼 알고리즘](#크루스칼-알고리즘)
    - [프림 알고리즘](#프림-알고리즘)


## 최소 신장 트리
---

* __최소 신장 트리(MST, Minimum Spanning Tree)란?__  
신장 트리 중에서 간선의 가중치 합이 최소인 신장 트리. 즉, 간선의 비용이 최소가 되게하면서 모든 노드를 연결하고 싶을 때 활용한다.  
> 신장 트리: 사이클이 생기지 않으면서, 모든 노드가 연결된 상태  
> `사이클 X`와 `모든 노드가 연결`이라는 조건을 잘 기억하자!  

* __최소 신장 트리 알고리즘__  
그래프에서 최소 신장 트리를 찾을 수 있는 알고리즘으로 `크루스칼 알고리즘`과 `프림 알고리즘`이 있다.  


## 크루스칼 알고리즘
---

* __크루스칼 알고리즘(Kruskal’s algorithm)이란?__  
최소값을 갖는 간선을 우선으로 하여 최소 신장 트리가 되도록 연결한다. 이 때, 사이클이 생기지 않게 연결한다. 간선을 오름차순으로 정렬하여 가장 낮은 비용의 간선 부터 사이클이 생기지 않는 간선만 채택하는 원리로 `그리디 알고리즘`을 기반으로 한다.
> 과정 요약: 간선 최소값 정렬 → 사이클 여부 확인(Find함수. path compression기법) → 두 조건을 만족하면, 연결(Union함수. union by rank기법)
![이미지1](https://jsim6342.github.io/assets/img/dev/algorithm/2021-05-02-dev-algorithm-mst-picture1.png)

* __Union  & Find__  
크루스칼 알고리즘의 사이클 여부 확인과 연결 작업에는 Union & Find라는 자료구조가 활용된다. 알고리즘이 구현되는 동안 Union으로 노드 간의 트리구조를 형성해 각 노드의 루트 노드를 빠르게 찾게 하여, 루트 노드 비교를 통해 사이클이 생기지 않게 하는 원리이다.  

  - __연결하는 작업. Union__  
    + Union 작업을 위해 rank dictionary 구조를 만들어 각 노드 마다 0으로 초기화한다.  
    + 간선 연결 시, 서로 높이(rank)를 비교해서 높이가 낮은 쪽을 높은 쪽에 붙인다.(높은 쪽에 낮은 쪽을 붙여야 전체적인 높이가 낮아지기 때문이다. 높이가 낮아져야 만들어진 트리를 Find 함수로 루트 노드를 탐색할 때 시간 복잡도를 줄일 수 있다.)  
    + 트리를 붙여줄 때, 높은 쪽에 있는 노드는 낮은 쪽의 부모 노드가 되므로, 'parent[낮은쪽 노드] = 높은쪽 노드'를 해준다.  
    + 만약 높이가 같다면, 둘 중 하나의 높이를 +1 해줘서 높여주고, 그곳에 낮은 쪽 높이를 붙인다.  

  - __사이클 여부를 확인하는 작업. Find__  
    + Union에서 연결한 같은 루트 노드에서 파생된 노드들은 같은 트리에 있다. 같은 트리에 있는 노드끼리 연결하면, 사이클이 발생한다.  
    + 즉, 각 노드의 루트 노드를 비교하여 같은지 확인하고, 다르면 사이클이 생기지 않으므로, 다를 경우 두 노드를 연결하는 방식이다.  
    + Find 작업을 위해 parent dictionary를 만들어 각 노드 마다 자기 자신의 노드로 초기화 한다.  
    + 루트 노드이면, 바로 반환. 그게 아니라면, 루트 노드가 될 때까지 Find 함수에 부모 노드를 넣어 재귀적 호출을 통해 루트 노드를 반환하는 원리로 Find 함수를 구현.  
    + 이 때, 루트 노드를 찾음과 동시에 parent dictionary에서도 해당 루트 노드를 찾은 루트 노드로 바꿔줌으로써, 다음 번에 또 다시 루트 노드를 찾는 과정을 줄일 수 있다. 이 과정을 `경로 압축`이라 한다. (경로 압축을 통해 루트 노드에 곧바로 직접 연결되는 트리 형태가 된다.)  
    + Union 작업을 거치며 생긴 트리 구조에 따라 parent가 부여되며, 처음엔 모두 루트 노드(자기 자신)로 초기화 되어있던 parent dictionary 값들이 Union으로 생긴 트리 구조에 맞게 변화되며, Find 함수가 정상적으로 작동하는 원리. (즉, 초기엔 Union이 발생되면서 트리 구조가 생기게 되고, 이를 통해 루트 노드 관계가 형성)  

![이미지2](https://jsim6342.github.io/assets/img/dev/algorithm/2021-05-02-dev-algorithm-mst-picture2.png)  
![이미지3](https://jsim6342.github.io/assets/img/dev/algorithm/2021-05-02-dev-algorithm-mst-picture3.png)  

* __크루스칼 알고리즘 구현__  
```python
parent = dict() # 부모 노드 값 저장
rank = dict() # 노드의 랭크 값 저장


def find(node):
    # path compression 기법
    if parent[node] != node: # 현재 노드가 루트 노드가 아니라면, 
        parent[node] = find(parent[node]) # 부모 노드를 계속 타고 올라가서 루트 노드를 부모 노드에 넣고,
    return parent[node] # 넣은 부모 노드(루트 노드)를 반환한다.


def union(node_v, node_u):
    root1 = find(node_v)
    root2 = find(node_u)
    
    # union-by-rank 기법
    if rank[root1] > rank[root2]:
        parent[root2] = root1
    else:
        parent[root1] = root2
        if rank[root1] == rank[root2]:
            rank[root2] += 1
    
    
def make_set(node): # 초기화 함수
    parent[node] = node
    rank[node] = 0

def kruskal(graph): # 사전을 인자로 받음
    mst = list()
    
    # 1. 초기화
    for node in graph['vertices']:
        make_set(node)
    
    # 2. 간선 weight 기반 sorting
    edges = graph['edges']
    edges.sort() # 여러 가지 sort 방법을 알맞게 사용하면 된다.
    
    # 3. 간선 연결 (사이클 없는)
    for edge in edges:
        weight, node_v, node_u = edge
        if find(node_v) != find(node_u): # find를 하여 루트 노드가 같지 않으면 union을 해준다.
            union(node_v, node_u)
            mst.append(edge)
    
    return mst # 간선에 최소 간선을 담아 반환
```  

* __크루스칼 알고리즘의 시간 복잡도__  
모든 간선을 최소 비용을 기준으로 오름차순 정렬할 때, 퀵 정렬을 사용한다면, 간선 e를 기준으로 O(E logE)  
두 정점의 최상위 정점을 확인하고, 서로 다를 경우 연결하는 Union & Find 자료구조는 union-by-rank 기법과 path compression 기법을 사용하여 시간 복잡도가 O(1)애 가깝다고 볼 수 있다.  
즉, 크루스칼 알고리즘의 시간 복잡도는 `O(ElogE)`라 할 수 있다.  


## 프림 알고리즘
---

* __프림 알고리즘(Prim's algorithm)이란?__  
시작 정점을 하나 선택하고, 해당 정점과 인접한 간선 중에서 최소 비용 간선을 선택하고, 선택된 간선과 연결된 정점에서 또 다시 최소 비용의 간선을 선택하는 방식으로 최소 신장 트리를 확장해 나가는 방식이다. 프림 알고리즘의 과정은 다음과 같다.
> 0. 최소 신장 트리를 저장할 리스트와 모든 간선 정보를 담을 dictionary를 만들어 저장한다.  
> 1. 연결된 노드들, 후보 간선들을 담는 연결된 노드, 간선 후보 리스트를 각각 만든다.  
> 2. 임의의 정점을 선택 후, 연결된 노드 리스트에 삽입.  
> 3. 선택된 정점에 연결된 간선들을 간선 후보 리스트에 삽입.  
> 4. 간선 후보 리스트에서 최소 가중치를 갖는 간선 부터 추출해서, 해당 간선이 연결된 노드 리스트에 들어 있다면, 스킵(사이클 방지). 들어 있지 않으면, 해당 간선을 선택하고 해당 간선 정보를 최소 신장 트리에 삽입  
> 5. 추출한 간선은 간선 리스트에서 제거  
> 6. 간선 리스트에 더 이상의 간선이 없을 때까지 4~5번을 반복한다.  
![이미지4](https://jsim6342.github.io/assets/img/dev/algorithm/2021-05-02-dev-algorithm-mst-picture4.png)  
![이미지5](https://jsim6342.github.io/assets/img/dev/algorithm/2021-05-02-dev-algorithm-mst-picture5.png)  
![이미지6](https://jsim6342.github.io/assets/img/dev/algorithm/2021-05-02-dev-algorithm-mst-picture6.png)  


* __프림 알고리즘 구현__  
사이클이 생기지 않게하기 위해, 연결하려는 다음 노드가 이미 연결된 리스트 내에 존재하는지 확인하는 작업을 거쳤다.
어떤 노드로 부터 파생된 인접 노드 사이의 최소 간선을 구하는 과정을 최소 힙으로 구현하였다.  
```python
from collections import defaultdict
from heapq import *

def prim(start_node, edges):
	mst = list() # 최소 신장 트리 간선을 저장, 출력을 위한 리스트 선언
	adjacent_edges = defaultdict(list) # 간선 정보를 저장하기 위한 리스트
	
  # 모든 간선들을 돌며, 비용과 연결 노드들을 adjacent_edges에 저장
	# dictionary 형태로 node가 주어지면 해당 노드와 연결된 간선들을 출력하기 위함
	for weight, n1, n2 in edges: 
		adjacent_edges[n1].appned((weight, n1, n2))
		adjacent_edges[n2].appned((weight, n1, n2))

	connected_nodes = set(start_node) # 시작 노드를 연결된 노드 리스트에 저장
	candidate_edge_list = adjacent_edges[start_node] # 간선 후보 리스트에 시작 노드와 연결된 간선들 저장
	heapify(candidate_edge_list) # 최소힙으로 우선순위 큐 형태로 변환

	while candidate_edge_list:
		weight, n1, n2 = heappop(candidate_edge_list)
		if n2 not in connected_nodes: # 연결하려는 n2 노드가 연결된 노드 리스트에 없다면,(사이클X)
			connected_nodes.add(n2) # 연결된 노드 리스트에 넣고,
			mst.append((weight, n1, n2)) # 최소 신장 트리에 해당 간선 정보를 넣는다.

			for edge in adjacent_edges[n2]: # 방금 연결한 n2 노드와 인접한 간선 정보를 돌며
				if edge[2] not in connected_nodes: # 간선 목적지 노드가 연결된 노드 리스트에 없으면(사이클X)
					heappush(candidate_edge_list, edge) # 간선 후보 리스트에 넣어 while문을 돌게 한다.
 
	return mst # 저장된 최소 신장 트리 반환
```  

* __프림 알고리즘 시간 복잡도__  
최악의 경우, while문에서 모든 간선에 대해 반복하여 O(E), 최소힙 구조를 유지하는데 O(logE)가 걸리므로, `O(ElogE)`이라고 할 수 있다.  