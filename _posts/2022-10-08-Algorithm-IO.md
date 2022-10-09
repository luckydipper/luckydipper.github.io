---
layout: post
title: "Algorithm 입출력 요약"
categories:
  - Algorithm
tags:
  - iostream
  - getline
  - cin
  - sync_with_stdio
last_modified_at: 2022-10-09
comments: true
---

# 목차
[1. io overhead 줄이는 tip](#1io-overhead-줄이는-tip) <br>
  * [1.1 c의 library와 multithread synchronization 끊기](#11-c의 library와-multithread-synchronization-끊기)<br>
  * [1.2 endl쓰지 말기](#12-endl쓰지-말기)<br>


[2. getline이 2개?](#22-getline이 2개?) <br>
  * [2.1 istream의 cin.getline](#21-istream의-cin.getline)<br>
  * [string getline()](#string-getline())<br>

[3. test case의 갯수를 주는 경우 vs 안 주는 경우](#3-test-case의-갯수를 주는-경우-vs-안-주는-경우)

---

## 1. io overhead 줄이는 tip
#### 1.1 c의 library와 multithread synchronization 끊기
```
// 해당 코드를 친 후에는 prinf와 scanf 사용 금지
io_base::sync_with_stdio(false)
cin.tie(nullptr);
```

[BOJ output 속도][1]  
[BOJ input 속도][2]  

#### 1.2 endl쓰지 말기
```
//endl은 overhead가 큰 연산임.
cout << endl;
```


## 2. getline이 2개?
**!!CAUTION!!**  
  cin과 getline을 같이 쓰면, cin에서 버퍼에 가져온 \n이 남아 있어, 다음 getline은 실행 되지 않는다. 그러므로 cin.ignore()을 쳐야한다. 
  > ref. [BOJ 9093](https://github.com/luckydipper/data_structure_algirithm/blob/main/baekjoon_algorithm/basic_lecture1/data_structure1/9093.cpp)
```c
int a;
cin >> a;
cin.ignore(int n, char dlim);
string sentense;
getline(cin, sentense);
``` 

#### 2.1 istream의 cin.getline
dlim의 앞까지 읽는다. 이때 c style의 array를 사용해서, '\0', EOL이 나올 때 까지 읽는다.
```c
istream& cin.getline(char* str, streamsize n)
istream& cin.getline(char* str, streamsize n, char dlim);
```

#### 2.2 string getline()
string과 같이 쓰이는 getline 함수이다.  
```
istream getline(istream& is, string str);
istream getline(istream& is, string str, char dlim);
```
##### ps. cin.get()으론 문자 1개 받을 수 있다.

## 3. test case의 갯수를 주는 경우 vs 안 주는 경우
반복 횟수를 주는 경우, 문제의 틀 반복문이 될 수 있다.  
[BOJ 1874][2]에서와 같이  

```c
while(origin.empty());
//보다는 주어진 숫자대로 반복하는 것이 좋다.  
```

반복 횟수를 주는 경우 주지 않는 경우  
```c++
int a, b;
while (scanf("%d %d",&a,&b) == 2)
while(cin >> a >> b);
```
[1]: https://www.acmicpc.net/blog/view/57

[2]: https://www.acmicpc.net/blog/view/56

[3]: https://github.com/luckydipper/data_structure_algirithm/commit/b366060cf4f9a761fb0e9550c174d58f7cd05564