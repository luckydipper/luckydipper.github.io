---
layout: post
title: "Dynamic Programing"
categories:
  - Algorithm 
tags:
  - Dynamic Programing
  - Devide and Conquer
  - Overlappiong subproblem
  - Optimal structure
  - Memoization
last_modified_at: 2022-11-05
comments: true
---

## Dynamic programming vs Devide and Conquer

|종류|차이점|공통점|
|DP와 D&C|---|큰 문제를 작은 문제로 나눠 푼다.|
|DP|작은 문제가 중복 가능하다. ex$)$ 숫자로 나눈다.|
|D&C|작은 문제들이 중복이 불가능하다.|

## DP로 풀 수 있는 문제의 속성 
1. overlapping sub problem
  - 작은 문제들의 형태가 겹친다. 
  - $$ Fibonacci(N) = Fibonacci(N-1) + Fibonacci(N-2)$$
2. optimal structure
  - 최적 부분 문제
  - 문제의 답이 작은 문제의 정답을 통해 구할 수 있다.
  - 서울에서 부산가는 가장 빠른 방법이 서울 -> 대전 -> 대구 -> 부산일 때, 대전에서 부산 가는 가장 빠른 방법은?

## Memoization 
**DP의 핵심이다.**    
푸는 중간에 정답이 변하지 않는 과정을 기록해 놓는다.  

## Implementation
top down으로 푸는 방법과, bottom up 방법으로 푸는 방법이 있다.  


top down은 recursion을 쓰는 방법으로 아래와 같다.  
```c++
int memo[100];
int fobonacci(int n)
{
  if(n < 1)
    return n;
  else if (memo[n] > 0)
    return memo[n];
  memo[n] = fibonacci(n-1) + fibonacci(n-2);
  return memo[n];
}
```

bottom up은 iteration을 쓰는 방법으로 아래와 같다.  
```c++
int memo[100];
int fobonacci(int n)
{
  if(n < 1)
    return n;
  for (int i = 2; i <= n; i++)
  {
    memo[i] = memo[i-1] + memo[i-2];
  }
  return memo[n];
}
```
### FAQ
1. 시간 차이는 알 수 없다.
2. python은 stack over flow가 자주 일어난다. sys.setrecursionlimit(1200)로 해결한다.
3. 둘중 하나만으로 풀리는 것도 존재한다.  

## 출처 
BOJ 기초 강의 1
