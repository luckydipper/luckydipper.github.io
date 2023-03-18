---
layout: post
title: "BFS"
categories:
  - Algorithm
tags:
  - flood fill
last_modified_at: 2023-03-10
comments: true
---

### BFS
graph를 넓이 기반으로 탐색함.  

대표유형
1. flood fill $($seed fill$)$
flood 알고리즘중 하나이다. flood 알고리즘은 graph의 각 부분에 물질이 분배되는 알고리즘이다. 마치 홍수가 났을 때 넘치는 것 처럼.  
flood 알고리즘은 네트워크나 그래프에 쓰인다. 그중 대표적인 것이 flood fill이다.  
flood fill은 같은 컬러의 경계를 따는 boundary fill과 대비된다.  
   
1.1 서로 같은 시작점이 여러개인 유형 : 
BOJ : 7576. 토마토  

1.2 서로 다른 색깔로 칠하는 유형: 
BOJ : 4179

2. implicit graph 
graph의 형태를 띄지 않지만 graph로 보면 편한 것.  
state를 graph의 node로 보면 편함.  
ex) occupancy grid map  


DFS에선 dfs 함수에 들어온 vertex는 방문하기 이전의 상태이어야 한다.  
BFS에선 queue에 push 되는 vertex가 discovered 된 상태이어야 한다.  

BFS를 응용하면
- 경로를 모두 저장하기 위해선 부모 방향으로 올라가는 array 기반 tree를 사용 할 수 있다.   
- 거리 계산 : discovered vector로 bool로 처리하는 대신에, distance로 int로 하는 것이 좋다. -1로 초기화 한 후 방문 순서를 최단거리를 계산할 수 있다.   
