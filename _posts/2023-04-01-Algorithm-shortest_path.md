---
layout: post
title: "Shortest Path Algorithm"
categories:
  - Algorithm
tags:
  - shortest path
last_modified_at: 2023-04-01
comments: true
---
## Floyd algorithm
모든 정점간의 최단 거리를 계산하는 알고리즘이다.  
음수인 cycle이 존재하면 안 된다.  
3단 for문을 통해 구현 할 수 있다.  
이때 **가장 바깥쪽 for문에 중간의 vertex를 둔다.**  
경로를 나타내기 위해선 `next table`을 이용한다. 
최단거리 table을 초기화 할 때, `nxt[from][to] = to`로 초기화 한다.  
k를 거쳐 가는 것이 더 효과적일 때에만 `nxt[from][to] = nxt[from][k]`  
base case 1. dist[from][to]가 INF이거나 0인 경우 : 연결 돼있지 않음.  
base case 2. from == to
## Dijkstra algorithm
어떤 정점에서 다른 정점까지의 최단 거리를 구하는 알고리즘.  
음수인 edge가 존재해선 안 된다.   
priority queue를 활용하면 더 효과적으로 구현 할 수 있다.  
weighted graph를 adj list인 `vector<pair<int,int>> p{weight, idx}`로 나타낸다.  
shortest distance table로 정점으로 부터의 거리를 저장한다.  
이 table을 array로 할지 priority queue로 할지에 따라 시간복잡도가 달라진다.  
경로를 나타내기 위해선 table이 갱신 될 때, 어디를 거쳐가면 되는지 기록해 둠.  
**priority queue에 들어가기 전에 dist가 확정이 된다. pq엔 vertex에 연결된 edge들이 들어간다.**
```
pre_table. 시작점에서 나에게로 올 때 직전에 어디에 방문했는가?  
```

## ps.  
음수인 edge가 있는 경우 다익스트라 알고리즘을 사용 할 수 없다.  
이때엔 bellman ford 알고리즘을 통해 구한다.  
시작점과 종점이 정해진 경우 A* 알고리즘을 사용할 수 있다.  
일반적인 코딩 테스트에선 다익스트라와 플로 이드 알고리즘을 사용한다.  

## ref
baaaarking dog 강의.  