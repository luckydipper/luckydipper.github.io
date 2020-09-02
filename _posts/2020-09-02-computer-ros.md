---
layout: post
title: "ros raspberrypi computer"
categories:
  - computer
tags:
  - ros
  - rasberrypi
  - raspbian
  - ubuntu
  - version
last_modified_at: 2020-09-02
comments: true
---

# ROS 프로그래밍을 하면서 힘든 점.

- 정의
ros란?<br>
ros는 로봇을 위한 다양한 패키지를 다운과, 이기종간의 통신을 쉽게 할 수 있게 한다.ros는 os(운영체제)와는 다르게 다른 운영체제 위에서 작동한다. 이것은 리눅스 운영체제와 잘 맞는다.<br>

- 주의 할 점
  1. 개발환경을 세팅하기 너무 어렵다.
  우선 linux 운영체제를 맞춰야한다. 이걸 제대로 하려면, 가상머신보다는 듀얼부팅을 해야한다. 이 과정에서 컴퓨터를 잘 못 건드리면, 포맷된다.
  
  2. 한국어를 택하지 말아야한다.
  linux에서 shell을 많이 이용했다. 한국어를 사용하면 오류 메세지가 한글로 나왔다. 그러면 이것을 다시 영어로 구글링해야한다. 한국어로 쳐도 잘 안 나왔다. 영어로 쳐도 안 나오는 것은 마찬가지이지만, ㅎㅎ

  3. 버전을 잘 맞춰야한다.
  우리가 사용하는 것이 desktop(computer)에 ubuntu, raspberry pi에 ubuntu mate 혹은 raspbian, ros이다. 이것들이 각각 호환하는 것이 다르다. (미치겠다 ^^) 운영체제가 계속 업데이트 되는데, LTS라고 써져있는 것이 오래 쓸 수 있다. 우분투는 20, 18, 16 버전이 LTS이고, ROS는 1에는 KINETIC melodic... 2에도 LTS가 따로있고, raspbian은 stretch jessi buster 같은 것이 있다. 나는 raspberry pi 4를 샀다. 그리고 desktop에는 ubuntu 16을 듀얼 부팅했다(16은 지원 기간도 끝났지만, 많은 한국어 자료가 여기 있었기 때문이다). 그리고 raspbian buster을 다운받았다. libboost에 문제가 생겼다. 그래서 raspbian의 버젼을 다운시키려고 했다. 그러나 raspberry pi 4는 buster만 지원한다는 것을 알게 됐다. 

정말 힘든 디버깅이지만 재미? 있는 것 같다. 그렇게 세뇌하고 있다. 하하 재밌다.
