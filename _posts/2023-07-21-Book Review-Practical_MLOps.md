---
layout: post
title: "ML Ops 실전 가이드 "
categories:
  - Book Review
tags:

last_modified_at: 2023-07-21
comments: true
---
![archtecture](/assets/img/Book review/practical_mlops.jpg)

## 1. MLOPs이란? 
MLOps란 DevOps의 머신러닝 버전입니다. DevOPs란 Development와 operation의 합성어입니다.  
Devops에선 "테스트의 자동화", "CI", "CD"등에 대해 다룹니다.  

대상 독자 :
솔직히 posix $($ unix,linux,mac $)$ shell에 익숙하고, python의 기계학습, docker, cloud 환경, 등등 배경지식이 좀 있으신 분이 읽어야 할 것 같습니다.  
모두 짧은 설명이 나오나, 부족한 느낌입니다. 

## 2. 책의 내용 
읽은 내용 중 기억에 남는 내용 위주로 적어 봤습니다.

automl : 데이터만 준다면 모델을 자동으로 만들어준다.  
종류로는 
- github action
  github에서 자동으로 테스트 해주고 해당 루틴을 실행 해 준다.  
- C++로 작업을 하는데 헷갈리는 용어들이 있었습니다.
  1. Sanitizers 
    코드를 실행하면서 분석하는 동적 분석기이다.  
  2. Linters 
    코드를 실행하면서 분석하는 정적 분석기이다.
  3. Static code analysis tools 
    clang-tidy와 같이 

- 엣지 디바이스 : 물리적으로 연산자원이나 인터넷이 제한적인 기기.
  google coral, tf lite  
 
- autoML, Docker, Cloud 기술, github action, AWS, AZure에서 mlops하는 법을 배웁니다.   

솔직히 어려워서 잘 못 읽었습니다.  

## 3. 책의 장점
1. 키워드 들로 어떻게 공부해야하는지 방향성을 볼 수 있었습니다.  

## 4. 책의 단점  
1. 처음 해보는 분야이고 번역서라 그런지 부드럽게 읽히진 않습니다.  
물론 제 독해력이 부족한 부분도 있습니다.  

"한빛미디어 \<나는 리뷰어다\> 활동을 위해서 책을 제공받아 작성된 서평입니다."
