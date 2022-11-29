---
layout: post
title: "Rotation representation"
categories:
  - Math Physics
tags:
  - Auler angle
  - Rotation vector
  - Angle axis
  - rotation matrix
  - quaternion
last_modified_at: 2022-11-20
comments: true
---
회전을 나타내는 대표적인 방법은 3가지이다.  

## 1. Euler Angle
개념 : x축과 y축 z축 방향으로 회전한 각도를 $\mathbb{N}^3$ 벡터로 나타낸다. roll pitch yaw  
장점 : 가장 직관적으로 회전을 나타낼 수 있다.  
단점 : 회전에 순서가 매겨진다. 그러므로 Gimbal lock 현상이 일어날 수 있다.  
예시 :  
z축 기준 $90^\circ$ 회전 후 x축 기준 $20^\circ$ 회전한 것과 y축 기준 $20^\circ$ 후 회전 z축 기준 $90^\circ$ 회전한 것이 같다.  

|서로 다른 회전이 순서를 바꾸면 같은 상태가 된다.|Gimbal lock 예시|
|![example-of-gimbal](/assets/img/Math Physics/Rotation representation/An-example-of-gimbal-lock.png)|![Gimbal_Lock_Plane](/assets/img/Math Physics/Rotation representation/Gimbal_Lock_Plane.gif)|


## 2. Angle axis $($rotation vector$)$
개념 : 기준축 $($reference axis$)$을 기준으로 단위 벡터$\boldsymbol u$와 $\theta$를 통해 회전을 표현한다.  
장점 : 나름 직관적으로 회전을 나타낼 수 있다.  
단점 : 실제 벡터에 회전을 적용시키기 위해서는 쿼터니언이나 행렬 형태로 바꿔야 한다.  
예시 :  

|Angle Axis|
|![angle-axis](/assets/img/Math Physics/Rotation representation/angle-axis.png)

## 3. Quaternion 
개념 : $p^{\prime} = q \otimes p \otimes q$으로 pure quaternion인 p의 위치를 회전시킨다.  
이때 회전시키는 q 들은 unit quaternion으로
장점 : 
단점 : 직관적으로 이해하기 힘들다.  

## 출처
https://stackoverflow.com/questions/9417246/quaternions-vs-axis-angle