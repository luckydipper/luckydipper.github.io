---
layout: post
title: "SLAM 장비 추천"
categories:
  - SLAM
tags:
  - SLAM KR
  - 장형기
last_modified_at: 2022-10-10
comments: true
---

## 컴퓨터 추천
### desktop
cpu : intel vs amd 가격 측면에서 amd가 우세. 하지만 SIMD, avx기술에서 intel이 우세  
GPU : Nvidia$($Geforce$)$ vs AMD$($Radeon$)$ opencv와 cuda를 위해선 NVIDIA 제품이 필요함.  

### mobile
Nvidia :  Jetson/ Nano/ TX2/ Xavier  
lattepanda : window 깔림 google coral연결 후 딥러닝 가능   
Intel NUC : 파워 필요.  
android 보다는 ios가 최적화가 잘 돼 있음.  

# 카메라 추천

## visual slam에서 쓰는 카메라 종류
#### 현업
- RGB 카메라 $($slam에서는 흑백 카메라가 적합함. but deeplearning backbone, hw가 rgb 기반임$)$ 단점 : 1.discrete 부분 interpolation. 그러므로 흑백이 좋을 수 있음. 2. 필터로 다른 빛은 통과 못하기 때문에 loss of brightness 3. more computation. processing 과정에서 곱하면서 loss도 올라 감. 4. resolution loss 
- 모노 카메라
- 멀티모달: 라이다, 레이더, 소나, 분광기 IMU, GPS
#### 학술
- 편광 카메라$($polarisation camera 빛 반사 억제 x$)$
- 적외선 멀티 하이스펙트럴 카메라 ex$)$ 오래된 그림 진품 가품 판명, 인공위성, 헤모글로빈 혈관, thermal 열화상. dataset
- 이벤트 카메라


## monocular
- webcam
- <mark> machine vision cammera. 국내 산업용 <b>FLIR</b>, Basler$($비싸$)$ 국내 전파 인증. 연구용이면 lds, Edmund optics, Allied vision </mark>
- DSLR$($Digital Single Lens Reflex camera$)$
- 카메라 모듈. body가 따로 필요하다
- 스마트폰 ios가 좋다
- event-camera : slam위한 카메라 비쌈

## stereo camera
- Stereolabs ZED/ ZED2/ ZED mini  
  - cuda dependency. minimum rtx 2070  
  - 가까운 이미지 초점 x  
- Intel Realsense T265
  - Fisheye 렌즈. 넓은 시야  $($visual slam 돼 있어.$)$

## rgbd cammera
- Microsoft kinect2 / Azure Kinect $($성능 좋음$)$
- intel realsense D415/ D435/ L515
  - <b>openVINO<b>



## 디지털 이미지란? 
silicon vs biological. silicon이 센싱이 우수하나, processing이 안 됨.  
**In camera processing, color science, prof Michacel Brown**
어두울 수록 초록색과 파란색이 잘 보이고, 밝을 수록 빨간색 처럼 밝은 색이 잘 보인다.
computer graphics에선 RGBA가 각 $2^8 = 0 $~$ 255$씩 할당 됨 $($A는 투명도를 뜻 함.$)$  

## RGB camera vs gray camera
slam에서는 gray 카메라를 더 많이 쓴다.  
rgb 카메라를 쓸 경우 seonsor 사이사이 linear ineterpolation을 해야 한다.  
또한 센서의 사이즈가 적어서 빛이 들어오는 양이 적다.  

- Sensor Resolution
VGA 640 x 480, HD, FHD, 4k..  
resoulution이 높을수록 좋은 것은 아니다. 
sensor resolution이 커질 수록 픽셀의 크기는 작아져 들어오는 빛의 양이 줄어든다.  
줄어든 빛을 살리기 위해 gain을 넣어도 noise가 올라 간다.  
높을수록 fps가 낮아져야한다

- sensitivity $($quantum efficieny$)$
작으면 적은 조명에도 잘보인다. 

- FPS

- Sensor size
크면 노이즈가 작아지고 빛이 많이 들어와 밝아진다.  

- noise
  - shot noise
  - read noise
  - heat stability

- Dynamic range
조명변화가 잦은 경우 잘 보여야 할 때 고려.

- sensor type
  - CCD $($charge couple device$)$
  - CMOS $($complementary metal ocide semiconductor$)$
  - CMOS가 slam에 자주 쓰임. 들고 다닐 수 있는 가벼운 카메라에 씀  
  - Rolling shutter effect 가 있음. 윗족 pixel부터 읽기 때문임.  
  - 그러므로 무조건 **global shutter**로 찍어야 함.

- Interface
  - usb 길이가 길면 ratency 심해짐. 5~10 
  - gigE$($gigibit ethernet$)$
  SATA, LAN, USB 전송 속도

- etc
  - frame size weight
  - power

- 드론에 들어가는 가벼운 카메라 위드로봇 카메라

### 렌즈 
- spatial resoulution
  - 얼마나 객제가 이미지 픽셀에 구체적으로 묘사되는지. 공간 측량의 가장 작은 단위로 측정 됨.

- contrast

- Distortion
rule of thumb, 국룰   
working distance를 field of view보다 2배 이상 크게 함.  
그래야지 망원 렌즈가 됨.  
광각렌즈는 광범위한 정보를 담을 수 있지만 외곡이 심함.  
realsense의 T265가 fish eye lens를 껴서 calibration함.  
- Perspective
- Depth of field

- 호환성
lens calculator, edmund otics사 이용.  
cmount, csmount에 맞춰서 렌즈 구매  

- 잘 되는지 MTF 이용  
Modulation Transfer Function  
점점더 얇아지는 선 1mm에 99개의 선을 90%의 확률로 분간한다.  
  

## 출처
[장형기님의 slam 강의][1]

[1]:https://www.youtube.com/playlist?list=PLubUquiqNQdOjoEXERd88E0PhWLnK1IDy
