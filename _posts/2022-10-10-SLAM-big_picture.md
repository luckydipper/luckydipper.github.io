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
  - SLAM
last_modified_at: 2022-10-21
comments: true
---
# 0. SLAM이란?
simultanious localization and maping 
#### 0.1 localization
map이 있을 때 여기가 어디인지 파악하기.
#### 0.2 mapping
내가 어디 있는지 알 때 지도 만들기.  
**지도 종류**
- grid map
- match map
- point cloud 
- semantic map
#### 0.3 motion model 
quaternion kinemetics 

#### 0.4 observation model 
distance, bearing 

#### frontend backend
frontend = factor graph  
backend = optimize factor map  
확률적인 graph 최적화 문제

# 사용 분야 
- 증강현실, 부동산, autonomous vehicle  

# 1. 비슷한 용어 정리
- Visual odometry = Slam - loop closure$($여기 와본 적 있어$)$
- Structure From Motion $($시간 제한 없이 천천히 만들자$)$
- path planning $($Decision making$)$
- photogrammetry 
- 3D reconstruction


# 2. 도구에 따른 분류
- Lidar + gpu lidar slam $($더 큰 스케일에서 할 수 있도록$)$
- Cammera + imu. Visual slam $($imu가 fps가 더 좋아서 이미지 사이에 curve를 그릴 수 있음. $)$
- Sensor fusion
- etc$)$ Sona, Radar

# 3. 접근에 따른 분류
- Deep slam
- filtering based ex$)$ EKF, Montecarallo
- graph based $\in$ optimization based

# 4. passive slam vs active slam
- active slam  
 ex) coastal navigation  
 active slam은 localization과 mapping의 성능을 극대화 하기 위한 optial한 trajectory를 계획하는 slam이다.  
- passive slam
 그 밖의 slam  

# 5. Offline slam$($Full SLAM$)$ vs Online SLAM
$x_t$는 t에서의 포즈이고 m은 맵 z와 u는 측정 값 과 제어값이다.  
- Offline, Full SLAM  
현재 포즈 대신에 전체 경로에 대한 사후 확룰 계산  
$$p(x_t, m|z_{1:t},u_{1:t})$$
- Online SLAM  
현재 포즈  
$$p(x_{1:t}, m|z_{1:t},u_{1:t})$$

|Full SLAM|Online SLAM|
|![full_slam](/assets/img/SLAM/22_10_8_big_picture/full_slam.png)| ![online_slam](/assets/img/SLAM/22_10_8_big_picture/online_slam.png)|


# 6. localization종류
#### 6.1 position tracking vs global localization vs kidnapped robot

#### 6.2 static environmnet vs dynamic environment

#### 6.3 passive localization vs active localizatoin


# 7. History 

## 7.1 Filtering base
#### 1990 EKF SLAM  
O$$(N^2)$$으로 늘어나서 큰 지역에선 사용할 수 없다.
#### 2002 FAST SLAM
구글웨이모에서 일하시다가, 강의 플렛폼 유다시티 만드신 세바스찬스런 교수님의 알고리즘.
#### 2005 Gmapping
#### 2011 Hector SLAM
#### 2015 cartographer 
google이 개발한 SLAM

## 7.2 Feature Based Visual SLAM
Feature 기반 SLAM은 CPU를 주로 사용한다.
#### 2003 mono SLAM
#### 2007 PTAM
#### 2015 ORB-SLAM
#### 2017 ORB-SLAM2
#### 2017 VINS-Mono
#### 2020 ORB-SLAM3

## 7.3 Direct SLAM
Direct SLAM은 이미지의 모든 부분을 사용한다.
#### 2010 DTAM
#### 2014 LSD-SLAM
#### 2015 SVO 
#### 2017 DSO !! DRON에 사용될 만큼 속도가 빠르다.
#### DM-VIO

## 7.4 RGM-D SLAM
#### 2012 KinectFusion
#### 2014 RTABmap
#### 2015 Elastic Fusion
#### Bundle Fusion

## 7.5 LIDAR SLAM
#### 2014 LOAM
#### 2019 ALOAM
#### 2019 HDL-SLAM
#### 2021 FAST-LIO

# 8. Deep SLAM
deep leanging을 왜 쓴ㄴ가? data로 표현하기 어려운 패턴을 학습시켜서 feature를 extract하기 위해서.
## 8.1 Trand
- super point + super glue를 통해 feature점을 얻는다.  
- NetVLAD를 통해 같은 sceane을 보고 있는지 loop closure할지 판단한다. 
- NERF를 이용한 Nice SLAM
- Detection과 Segmentation으로 움직이는 사물의 frame을 뺀다.
## 8.2 Deep slam 문제점
- Data 부족
- Geometry는 자명함.  
- geometry prior를 학습시키기 위해 undeepvo, depth poses, trantanvo, d3vo등이 나옴.   
## 8.3 Spatial AI. 
CVPR2020 Andriew 교수님의 From SLAM to Spatial AI 강의에서 왔다.  
SLAM의 본질로 들어가자.
- level 4 object level
- level 3 semantic understanding
- level 2 dense map
- level1- pose + sparse map
## 출처
- SLAM KR의 전액 기부 slam 강의
- probabilistic robotics
- SLAM AI 22.10.18