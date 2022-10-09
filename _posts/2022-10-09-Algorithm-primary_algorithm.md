---
layout: post
title: "백준 알고리즘 기초 수학 1"
categories:
  - Algorithm
tags:
  - modular
  - GCD
  - euclidian algorithm
  - prime number
  - 
last_modified_at: 2022-10-09
comments: true
---
[comment]: <> (만약 용량이 적으면, 굳이 목차 넣지 말자.)

## 1. modular operation
가끔 문제에서 큰 숫자가 나오면 modular를 물어봄.  
만약 빼기를 할 경우. 마이너스가 나올 수 있음.  
**$(a-b) % c 에서 음수가 나올 경우 + c**

## 2. Greatest Common Devider
### 2.1 a~b까지 나눠봐.
O$($N$)$  
### 2.2 euclidian algorithm
GCD$($a,b$)$ == GCD$($b, a%b$)$
O$($logN$)$  

## 3. prime number
### 3.1 N -> prime number?
#### 3.1.1 2~N-1까지 조사
O$($N$)$
#### 3.1.2 2~N/2까지 조사
O$($N$)$
#### 3.1.3 2~ $\sqrt{N}$까지 조사
O($$\sqrt{N}$$)

### 3.2 N 이하 모든 prime number
#### 3.2.1 N이하의 모든 수에 3.1적용
O($$N\sqrt{N}$$)

#### sieve of Erastosthesnes 
어떤 수의 제곱보다는 작은수는 지워져 있다는 것이 보장됨.  
ex$)$ 10x10 grid에서 prime number들을 찾으려면, 11x11은 121이므로 11 이전 까지만 하면 됨.  

## 4. Goldbach's conjugation
- 2보다 큰 모든 짝수는 두 소수의 합으로 표현 가능하다
- 5보다 큰 모든 홀수는 세 수수의 합으로 표현 가능하다.
$10^{18}$이하에선 참임이 증명 돼있음.  
$$ N = a + b 일 때 $$  
$$ a \in prime, N-b \ \ prime?$$

## 5. Factorial
10! 300만임. 정말 빠르게 늘어난다. c++ java에선 담지도 못함. 0의 갯수나 log scale 같은 것으로 다운시켜 푼다.  

**signed int는 최대 21억**
