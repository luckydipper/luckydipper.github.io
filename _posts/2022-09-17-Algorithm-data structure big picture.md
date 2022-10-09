---
layout: post
title: "Data structure big picture"
categories:
  - Algorithm
tags:
  - ADT
  - implementation
last_modified_at: 2022-09-17
comments: true
---

# Abstract Data type vs Implementation
자료구조를 공부할 때 제일 중요했던 것은 ADT$($추상 데이터타입$)$와 implementation$($구현$)$을 구분하는 것이다.  
아래는 간단한 자료구조의 예시이다.  
![image](/assets/img/computer/algorithm/data_structure_big_picture/sort_of_data_structure.png)
우리는 선형 자료구조와 비선형 자료구조 위주로 공부한다.  

## Linear data structure
stack queue deque는 선형 ADT이다.  
순차리스트$($array$)$와 linked list는 ADT를 구현 하는 방법이다. 

stack과 queue는 대부부분 array를 이용해 구현한다.  
원소들 사이에 삽입하거나 삭제하는 연산이 불필요하기 때문이다.  
하지만 동적 할당으로 정해진 크기를 넘어가면 다시 크게 할당 받도록 구현한다. 

ex$)$ std::vector 기반으로 만든 std::queue 와 std::stack

|Stack|Queue|
|![image](/assets/img/computer/algorithm/data_structure_big_picture/vector.png)|![image](/assets/img/computer/algorithm/data_structure_big_picture/circular_queue.png)|

deque는 doubly linked list로 구현한다. 앞과 뒤에 삽입과 추출이 일어나야하기 때문이다.  
**stack** : recursion, DFS, DP$($Dynamic Programing$)$, etc...  
**queue** : OS의 jop queue, 기다리는 사람, etc  
**dequeue** : 앞 뒤로 넣어야 할 때 사용  
**linked list** : element들의 erase와 insert가 많을 때 사용    

## Nonlinear data structure
tree, graph, priority queue, hash table, set은 ADT이다. 

일반적인 binary tree는 node structure를 이용해 구현 할 수 있다. $($BST$)$  
heap을 구현할 때 쓰는 특수한 complete binary tree는 array를 통해 구현한다.  
graph는 Adjacent matrix나 list를 통해서 구현 할 수 있다.  
priority queue는 heap을 통해서 구현 할 수 있다.  
이 밖에도 hash table은 array에 중복된 값이 있으면 다음 부분에 저장하는 형식으로 만들어진다.  

# 정렬 알고리즘 구현 탐색 알고리즘 구현 
정렬 알고리즘 정리 사이트  
https://hyo-ue4study.tistory.com/68  
탐색 알고리즘 정리 사이트  
https://bba-dda.tistory.com/21  


# 결론
이미 잘 알려진 자료구조, 한번씩 구현해 본다.  
익숙해지면 STL로 가져다 쓰는 것이 좋다.  