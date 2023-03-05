---
layout: post
title: "BOJ 단계별 풀어보기 사칙연산"
categories:
  - Algorithm
tags:
  - BOJ 단계별 풀어보기
  - 입출력과 사칙 연산
  - setprecision
  - sprint
  - integer overflow
last_modified_at: 2023-03-05
comments: true
---
본 포스팅은 'BOJ 단계별 풀어보기'의 입출력과 사칙 연산에서 기억 할 부분을 정리했다. 

### integer overflow
int의 overflow 경계값은 21억이다. $($메이플 메소 최대 값이 21억이었다.$)$  

### 부동소수점
float의 유효 숫자는 7.  
double의 유효 숫자는 16이다.  
그러므로 double을 쓰자.

### string to int 
string STL을 include 한 후, 
```
to_string(type) 
stoi, stod, stoll
string_obj.c_str(); //return c style string
itoa() // msvc 컴파일러에서만 가능

// 대안
int num = 123;
char str[5];
sprintf(str, "%d", num); // integer to c string
printf("%s\n", str);

```

### Decimal place value 
Decimal은 10진수 또는 prime number$($소수$)$이다.  
```
int getNPlaceValue(int num,int N){
    return num / (int)pow(10,N-1) % 10;
}
```

### Decimal place
```
std::cout << std::setprecision(5) <<f ;
```
