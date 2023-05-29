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
last_modified_at: 2022-05-29
comments: true
---

## 1. Dynamic programming
cache를 사용하여 빠르게 수행하는 알고리즘.  

## 2. DP로 풀 수 있는 문제의 속성 
1. overlapping sub problem
  - 완전탐색 할 때 문제들의 형태가 겹친다. 
  - $$ Fibonacci(N) = Fibonacci(N-1) + Fibonacci(N-2)$$
2. optimal structure
  - **현재까지 어떤 선택을 해왔던지, 앞으로 최적의 선택을 한다면 최적의 답을 얻을 수 있다.**  
  - cache를 설계 하는데 핵심 원칙이다. 
  - 문제의 답이 작은 문제의 정답을 통해 구할 수 있다. 

예시) BOJ 2156. 포도주 시식을 **비효율적**으로 푸는법  

```c++
#include<bits/stdc++.h>
using namespace std;
int n;
int arr[10002];
int cache[10002][3];
// i를 마셨을 때, i는 j번 연속으로 마셨다.
// 1~i-1까지 마실 수 있는 최대 음료량을 반환한다.
int max_drink(int i, int continous_1){
    if(i <= 0 || continous_1 >= 3) return 0;

    int& ret = cache[i][continous_1];
    if(ret != -1)
        return ret;
    
    int max_before = max_drink(i-1, continous_1+1);
    for(int jump = 2; jump <i; jump++){
        max_before = max(max_before, max_drink(i-jump, 1));
    }
    return ret = arr[i] + max_before;
}  
int main(){
    cin >> n;
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    for(int i = 1; i <= n; i++)
        cin >> arr[i];
    
    memset(cache, -1, sizeof(cache));

    cout << max_drink(n+1,0);
}
```
위의 코드의 문제점은 지금까지 연속으로 먹은 횟수를 기록하고 있기에 더 비효율적으로 동작한다.  
총 $O(N^2)$의 시간복잡도로 설계했다.  
이를 최적화해서 아래와 같이 쓰면,  

```c++
// D[i] : 1~i까지 먹은 최대의 와인양
D[i] = max(D[i-1], D[i-3] + arr[i-1] + arr[i], D[i-2] + arr[i]);
```
$O(N)$으로 풀 수 있을 것이다. 


## 3. 유형
1. 최적화  
- 모든 경우의 수를 고려해서 그 중 최고의 답을 찾는 알고리즘 
  -  완전 탐색 알고리즘으로 설계. 
  -  **전체 답의 점수를 반환 하는 것이 아니라, 앞으로 남은 선택들에 해당하는 점수를 반환하도록 만듬.**
  -  **optimal sub structure에 의해 입력에 이전의 선택에 관련된 정보가 있다면 필요한 것만 남긴다.**
2. 경우의 수, 확률
- 지수적으로 증가하는 경우의 수를 세는 방법. 
  - 완점 탐색 시, ME, CE하게 나눠서 찾는다.  
  - 경우의 수는 exponential 하게 증가할 것이므로 `long long`을 쓰는 것이 좋음.  
  - **이전 조각에서 결정한 요소들에 대한 입력 값을 없애거나 변형해서 줄인다.**  
  - **앞으로 남아있는 조각들을 고르는 경우의 수만 반환 한다.**


## 4. Memoization 
**DP의 핵심이다.**    
- 푸는 중간에 정답이 변하지 않는 과정을 기록해 놓는다.
- **항상 cache에 들어가기 전에 base case를 처리해 줘야 한다.**  

```c++
if(out_of_bound(i,j) || base_case(i,j))
  return 1;
```  

- 아래와 같은 방식으로 cache를 쉽게 초기화 가능하다.  

```c++
//memset은 cache를 0이나 -1로만 초기화 할 수 있다. 
cache[99][34];
memset(cache, -1, sizeof(cache));
```

- reference로 cache를 그때 그때 typing 하지 않는다.  

```c++
int& ret = cache[i][j];
```

- return 할 때 path compression 경로 압축 한다.  

```c++
return ret = max(cache[i-1] + cache[i-2], cache[1]);
```

## 5. 시간 복잡도 분석
naive하게 부분 문제의 갯수 x 하나의 부분 문제를 푸는데 필요한 반복 수  
즉, 함수가 불린 횟수 x 함수 안에서 반복 수  

## 6. Implementation
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

## 7. Dynamic programming vs Devide and Conquer

|종류|차이점|공통점|
|DP와 D&C|---|큰 문제를 작은 문제로 나눠 푼다.|
|DP|작은 문제가 중복 가능하다. ex$)$ 숫자로 나눈다.|
|D&C|작은 문제들이 중복이 불가능하다.|

## 8. 출처 
- BOJ 기초 강의 1
- 알고리즘 문제 해결 전략
