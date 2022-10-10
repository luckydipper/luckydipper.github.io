---
layout: post
title: "SLAM 큰그림 용어 정리"
categories:
  - SLAM
tags:
  - SLAM KR
  - 장형기
  - Structure From motion
  - Visual Odometry
  - Path Planning 
  - Photogrammetry
  - Image Stitching
  - 3D reconstruction
  - probabilistic robotics
last_modified_at: 2022-10-10
comments: true
---

## 1. 비슷한 용어 정리
- Visual odometry = Slam - loop closure$($여기 와본 적 있어$)$
- Structure From Motion $($시간 제한 없이 천천히 만들자$)$
- path planning $($Decision making$)$
- photogrammetry 
- 3D reconstruction


## 2. 도구에 따른 분류
- Lidar + gpu lidar slam $($더 큰 스케일에서 할 수 있도록$)$
- Cammera + imu. Visual slam $($imu가 fps가 더 좋아서 이미지 사이에 curve를 그릴 수 있음. $)$
- Sensor fusion
- etc$)$ Sona, Radar

## 3. 접근에 따른 분류
- Deep slam
- filtering based ex$)$ EKF, Montecarallo
- graph based $\in$ optimization based

## 4. passive slam vs active slam
- active slam  
 ex) coastal navigation  
 active slam은 localization과 mapping의 성능을 극대화 하기 위한 optial한 trajectory를 계획하는 slam이다.  
- passive slam
 그 밖의 slam  

## 5. Offline slam$($Full SLAM$)$ vs Online SLAM
$x_t$는 t에서의 포즈이고 m은 맵 z와 u는 측정 값 과 제어값이다.  
- Offline, Full SLAM  
현재 포즈 대신에 전체 경로에 대한 사후 확룰 계산  
$$p(x_t, m|z_{1:t},u_{1:t})$$
- Online SLAM  
현재 포즈  
$$p(x_{1:t}, m|z_{1:t},u_{1:t})$$

|Full SLAM|Online SLAM|
|![full_slam](/assets/img/SLAM/22_10_8_big_picture/full_slam.png)| ![online_slam](/assets/img/SLAM/22_10_8_big_picture/online_slam.png)|


## 6. localization종류
#### 6.1 position tracking vs global localization vs kidnapped robot

#### 6.2 static environmnet vs dynamic environment

#### 6.3 passive localization vs active localizatoin

## 출처
- SLAM KR의 전액 기부 slam 강의
- probabilistic robotics