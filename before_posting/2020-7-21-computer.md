---
layout: post
title: "파이썬 클래스 이해하기 python class"
categories:
  - computer
tags:
  - class
  - attribute
  - object
last_modified_at: 2020-07-21
comments: true
---
<img src="/assets/img/hobby/slime.jpg">
python을 배우면서 가장 어려웠던 것이 뭐였을까?<br>
당연히 class였다! 지금은 익숙해 졌지만 처음에는 그 개념이 잘 와닿지 않았다.<br>
그래서 인터넷에 구글링한 정보를 모아서, 나만의 언어로 정리해보고자 한다.<br>
<br>



# class 이해방법

1. 새로운 자료형, 나만의 타입을 만들다. {클래스가 무엇인가? & 필수 용어 정리}
2. 함수와 데이터를 묶는다. {왜 쓰는가?}
3. 내가 만든 type의 연산방식을 정한다{생성자 & 필수 용어 정리2}
4. 너와 나는 class가 달라~{추가적 기능.overriding, overloading, inherit, private}



<br>
<br>
<br>

## 1. 새로운 타입을 만든다.
* "class는 새로운 인스턴스와 객체를 만든다."이 말은 너무 어려워보인다.<br>
* 쉽게 말하자면, <mark>class는 나만의 type을 만든다는 것이다.</mark><br>
예를 들어보자.<br>
```python
변수1 = "안녕?"
#변수1에는  "안녕?"가 문자형의 자료형으로 들어가 있다.
변수2 = [1,2,3,4]
#변수2에는 [1,2,3,4]가 리스트 자료형으로 들어가 있다.

print(type(변수1),type(변수2)) # str과, list가 나올 것이다.
```

* 위의 코드에서 보면 "안녕?"는 문자열의 instance, 즉 문자열의 한 <mark>예시</mark>이다.<br>
  (instance는 예시라는 뜻을 가지고 있다.) 예시라는 한국어가 있지만, 코딩할 때는 인스턴스라고 부른다.<br>
* <mark>파이썬의 모든 것들은 인스턴스로 만들어져있다.</mark>그러나 우리는 인스턴스라는 용어 대신 <mark>객체</mark>라는 용어를 쓴다.<br>
  class로 만들어진 instance(예시)라고 관계를 나타낼때는 instance라는 용어를 쓰고, 그냥 instance를 가르킬때는 객체라는 용어를 쓴다.<br>
  사실은 비슷한 말이라서 혼용해서 쓰기도 한다. <br>
<br>
<br>
<br>

## 필수 용어정리
더 자세한 용어를 설명하기 위해 다음 예시를 들어보자.<br>
용어가 많아보이지만 형광색으로 친 용어들은 모두 아는 것이 좋다.<br>
```python
class 슬라임:
  구성물질 = 액체 #self가 붙지 않은 것들은 class변수라고 한다.
monster1 = 슬라임()
monster2 = 슬라임()

print(monster1.구성물질,monster2.구성물질,type(monster1))
```
나는 class로 나만의 슬라임 타입을 만들고, 그 예시인 monster1을 만들었다.<br>

1. 구성물질 이것을 <mark>class변수</mark>라고 한다.<br>
  class로 나만의 type을 만들때, 모든 슬라임이 <mark>공통으로</mark> 갖는 요소를 저장하고 싶다. 이때 우리는 class변수를 사용한다.<br>
  class안에 그냥 변수를 선언하면, class변수가 만들어진다. 이것은 뒤에 나오는 instance변수와 구별된다.<br>
  ```monster.구성물질```를 통해 부를 수 있다. <br>
  ```print(monster.구성물질)```로 원래 설정 해놨던 class변수를 가져올 수 있고,<br>
  ```monster.구성물질 = 고체```로 고칠 수도 있다.<br>
  ```monster.새로운특성 = 까칠함```까지 새로운 class 변수를 지정하여 넣을 수도 있다.<br>


2. ```self.hp = 10``` {우리는 이것을 <mark>instance변수</mark>라고 한다.}
  class변수로 모든 슬라임의 공통 요소를 만들어 줬다면, <mark>개인적인 요소</mark>를 넣어줘야 한다.<br>
  - self 이것이 쓰인 이유가 궁금할 것이다. self는 instance를 가르키기 위해서 넣은 것이다.<br>
  <b>그림 첨부 예정</b>

  이 instance 변수는 반드시```__init__(self)```이라는 <mark>생성자 함수안에 넣는 것을 추천한다.</mark><br>
  안 좋은 예시를 보여주겠다.<br>
  ```python
  class 슬라임:
    self.hp =10
  monster = 슬라임()
  print(monster.슬라임)
  ```
  이렇게 쓸 수 있다. 하지만 이것은 class 변수랑 다를 것이 없다. 호출하는 방법도, 사용하는 방법도, 써넣는 방법도 class변수랑 다를 것이 없게 된다.<br>
  하지만 ```__init__(self)```이라는 함수에 넣는 순간 아래와 같아진다.<br>
  ```python
  class 슬라임:
    def __init__(self,hp): #클래스 안의 함수를 method라고 부른다. method중에서 __init__함수는 시작할 때 호출된다. 
      self.hp = hp    #이 줄이 가장 중요하다. 이렇게 instance변수에 받은 값을 대입 해줘야 한다.
      print("쿠후훗 몬스터가 생성되었다!!")
  monster1 = 슬라임(10)
  monster2 = 슬라임(12)
  ```
  위와 같이 몬스터가 생길 때마다. print할 수 있다.<br>
  즉, 나만의 타입을 만들 때, <mark>필요한 각각의 데이터를 instance를 만들 때부터 줄 수 있다.</mark><br>
  instance변수와 ```__init__(self,)

* def 귀여운척하기(self): {class 안에 정의된 함수를 <mark>매서드(method)</mark>라고 부른다.}
* def 점프하기(self):

```python
class 슬라임:
  구성물질 = 액체 #self가 붙지 않은 것들은 class변수라고 한다.
  self.hp = 10 #self. 으로 시작하는 것들을 instance변수라고 한다. 하지만 이렇게 쓰면 의미가 없다.
  def 귀여운척하기(self): #클래스 안에 정의된 함수를 method라고 부른다. 항상 self를 매계변수로 넣어주자.
    print("아잉~")
monster1 = 슬라임()
```
<br>
<br>

## 2. 함수와 데이터를 묶는다.
클레스를 만드는 이유는 객체지향적으로 프로그래밍하기 위해서이다.<br>
객체지향, 객체를 지향한다는 것은 <mark>데이터와 함수를 묶는다</mark>는 의미가 있다.<br>



아래 예시를 들어보자.
나는 게임을 만들 것이다.

## 3. 대수구조를 만든다.
대수구조