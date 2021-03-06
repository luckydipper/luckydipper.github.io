---
layout: post
title: "읽기 좋은 코드가 좋은 코드다."
categories:
  - Book Review
tags:
  - 읽기 좋은 코드가 좋은 코드다.
  - The art of readable code
last_modified_at: 2021-07-03
comments: true
---
#### 1). 코드는 이해하는데 들이는 시간을 최소화 시켜야한다.
<br>

## part1. 표면적 수준의 개선
#### 2) 이름에 정보 담기.
<mark>변수의 이름은 정보를 설명하는 공간이다.</mark> <br>

- 따라서, 명확하게, coding convention을 따른다.<br>
```
C++에서 google의 coding convention은 다음과 같다.
class      이름 : PascalCase (upper camel)
function   이름 : camelCase  (lower camel)
variable   이름 : snake_case
member var 이름 : snake_case_
const var  이름 : SNAKE_CASE
```

- 단어를 구체적으로 써야한다.<br>
ex) get : 짧은 시간 -> fetch, retrieve,download.<br>
ex) start -> launch, pause, restart, kill과 같은 느낌으로.<br>

- <mark>좁은 범위의 변수는 짧게, 광범한 변수는 길게 설정한다. </mark><br>

- 각 변수에 단위를 넣거나 추가적인 정보를 넣는다.<br>
ex) time_ms, unsafe, trusted_password<br>

#### 3) 오해하기 쉬운 이름.

- 경계를 포함하는 <mark>한곗값</mark>에는 min과   max를  사용한다.<br>
- 경계를 포함하는 <mark>범위  </mark>에는 first와 last를 사용한다.<br>
- 경계를 포함하고 배제하는 <mark>범위(마지막 미포함)</mark>는 begin과 end를 사용한다. <br>
- boolean을 저장하는 변수는 is has shoud와 같은 단어와 쓰고, 변수 이름은 긍정적인 것이 좋다. (부정의 부정이 일어날 수 있다.)<br>

#### 4) 미학
- 비슷한 일을 하는 코드는 비슷하게 쓴다.<br>
- 연관된 코드는 comment와 함께 <mark>하나의 block으로 띄어써서 묶는다.</mark> <br>
- <mark>코드의 열을 맞춰라 </mark>
- 변수의 설정 순서가 A,B,C이면, 다른 곳에서도 A,B,C로 써야한다.<br>

#### 5) 주석을 달아야 하는 대상
- 부족한 이름을 보충설명하는 주석을 달지 마라, 이름을 고쳐라 <br>
- 감독관 입장에서 생각을 기록하라.<br>
ex) 구조적 <mark>결함이나 큰흐름 위주로 기록</mark>하라.<br>
ex) <br>
```
// TODO  : 내일 마감이니깐, 이 기능 필요함.
// todo  : 이 부분 다르게 하면 더 좋을 듯.
// FIXME : xxx 부분 하다가, xx 오류나서 때려침.
// XXX   : 큰 위험성이 있다. 이거 파라미터를 이상하게 부르면, 1시간 걸린다.
```
- 상수는 만드시 설명하자. <br>
- 다른 사람들이 궁금해 할 지점을 타인의 관점에서 생각해보자.<br>

#### 6) 명확하고 간결하게 단다.
- 대명사<br>
- 입출력을 간단한 예시로 설명한다. <br>
- 만약 이름을 잘못 지었다면, function call 할때, <mark>comment로 parameter를 알려줄 수 있다.</mark>
```
check(/*time_ms = */ 10, )
``` 
- 모듈이나 함수의 전체적인 의도를 표시하자.<br>
<br>

## part2. 루프와 논리를 단순화
#### 7) 읽기 쉬운 흐름제어
- 조건문의 parameter 순서는 왼쪽을 유동적으로 오른쪽을 고정적으로 넣는다.<br>
- 삼항연산자 ? a:b는 간단해 지는 경우에만 사용한다.<br>
- do while은 추천하지 않는다 <br>
- <mark>중첩을 최소화 시켜서 선형적 구조로 만든다.</mark><br>
ex) 함수의 <mark>중간에 return 하거나, 중첩문의 continue를 통해서</mark> 선형적으로 만든다. <br>

#### 8) 거대한 표현을 잘게 쪼개기.
- 설명변수와 요약변수를 활용한다. <br>
- 복잡한 논리 조건은 작은 if로 나눠서 생각할 수 있다. <br>
ex) ||, &&를 따로 함수로 빼서 2개의 if로 쓰면 더 좋을 수 있다. <br>
ex) overlab range(); 느낌으로<br>

#### 9) 변수와 가독성
- 필요없는 변수는 없앤다. <br>
- 접근의 범위를 줄인다. static, scope, 변수의 위치 선정을 통해 어디에 쓰이는지 구분한다. <br>
ex) if 안에서 쓰는 변수는 if 안에서 설정한다.<br>
- const와 같이 바뀌지 않는 값을 선호하라. <br>
<br>

## part3. 코드 재작성
#### 10) 상관 없는 하위 문제를 추출하라.
- 함수를 구성할 때, 목적과 관계없이 복잡하면 추출해서 따로 만든다.<br>
- 읽을 때 너무 자세히 알 필요 없으면 추출해서 만든다.

#### 11) 한번에 하나만
- 말로 논리적으로 써본 후, 문단별로 분리 가능한지 생각해보자.<br>

#### 12) 생각을 코드로 만들기
- 함수의 과정을 <mark>영어로 설명</mark> 해본 뒤, 코드로 작성해보자.<br>

#### 13) 코드 분량 줄이기
가장 읽기 쉬운 코드는 아무 것도 없는 코드이다. <br>
- 기능 구현에 집착하지 말자. <br>
- 질문을 던지고, 나누어서 분석하자. 가장 단순한 형태의 문제는 무엇인가? <br>
- 필요 없는 기능은 제거하고 overengineering 하면 안 된다. (유지보수 테스트 힘듬)<br>
<br>

## part4. 선택된 주제들
#### 14) test!
- 유지보수 하기 쉽고 이해하기 쉬운 interface를 만든다.<br>
- tip : C++ boost library에서 assert 추가 기능을 이용하면 좋다.<br>