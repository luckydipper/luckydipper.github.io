---
layout: post
title: "DFS를 이용한 algorithm"
categories:
  - Algorithm
tags:
  - Topology sort
  - Euler circuit 
  - Euler trail
  - cut vertex
  - cut bridge
  - 2 SAT
  - cycle 여부
  - cross backward forward tree edge
  - Strongly connected component
  - DFS spanning tree
last_modified_at: 2022-12-25
comments: true
---

### 1. Topology Sort
어떤 node들이 종속되어 있을 때, 어떤 순서대로 node를 방문해야 종속에 의한 문제를 풀수 있을 까?  
정답이 유일 하지 않다.  
사용 예시 : 순서가 중요한 곳에서  
컴파일하고 링킹 하는 과정, 교육 과정 등등...  
푸는 방식에는 두가지가 있다.  
첫째는 DFS를 사용하는 방식이다.  
둘째는 kahn's algorithm을 통해 BFS를 이용하는 방식이다.  
kahn's algorithmd에 따르면 indegree가 0인 node를 찾는다.  
두 가지 방식으로 풀 수 있다. DFS를 이용한 방식과 kahn's alogirthm을 이용한 방식이 있다.  
step 1. 순서가 정해진 dependency graph를 그린다.  
step 2.  

graph에서 독립된 vertex들을 queue에 넣으면서 삭제함.  
이 때 종속된 vertex들의 indegree를 낮춤. indegree가 0이 된것을 다시 queue에 넣음.  