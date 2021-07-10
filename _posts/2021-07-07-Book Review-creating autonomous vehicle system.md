---
layout: post
title: "자율주행 자동차 만들기 chapter 1"
categories:
  - Book Review
tags:
  - 자율주행 자동차 만들기
  - creating autonomous vehicle system
  - chapter 1
  - 자율주행 개요.
last_modified_at: 2021-07-07
comments: true
---

## 자율 주행에 관한 개요를 볼 수 있는 상당히 좋은 책 같다.
### chapter 1. 자율주행 자동차의 개요.
자율 주행 시스템은 크게 2가지로 나뉜다.<br>
local {client} 부분과 server{cloud} 부분으로 나뉜다.<br>

1) client Algorithm<br>

| 센싱    | 인지             | 의사결정           |
| GPS/IMU | Localization     | action prediction |
| LiDAR   | Object detection | path plan         |
| Camera  | Object tracking  | obstacle avoidance|

2) client system<br>

| HW | OS |

3) cloud computer<br>

| HD map     | model training |
| simulation | data storage   |

<br>

### 1) Client Algorithm.<br>
##### 1-1) sensing<br>
sensing에 사용되는 센서는 대표적으로 4종류이다.<br>

##### 1-1-1) GPS/IMU <br>
GPS는 GNSS 위성에서 데이터를 받아오는 센서이다. 몇m의 오차가 있고, update 주기가 상대적으로 느리다.<br>
IMU는 가속도 센서로 오차가 누적된다. GPS와 <mark>칼만필터</mark> 로 오차를 줄여서 사용된다.<br>
이 두 sensor를 이용하여, 대체적인 localization{현재 어느 위치에 있는지}를 알아낸다.<br>

##### 1-1-2) Lidar <Br>
laser를 활용한 sensor이다. 레이저가 돌아오는 시간을 계산하여, 각점의 길이를 계산한다.
localization, mapping, HD map{입체 지도} 제작, 장애물 감지 등에 사용된다.<br>
매우 정확하지만, 비싸다.<br>

##### 1-1-3) Camera <br>
객체 인식 추적 등등 에 사용된다. <br>
현재 개발된 자율 주행 자동차는 안전을 위해 1080px의 카메라를 8대 이상 사용한다.<br>

###### 1-1-4) rader & sonar<br>
초음파 탐지 센서이다. <br>
최후의 수단으로 제어 장치와 연결되어 있다. <Br>

<br>

#### 1-2) recognition
##### 1-2-1) Localization
Localization은 현재 자동차가 어디있는지 계산하는 것이다. <br>
대표적으로 3가지 방법이 있다.<br>
1. GPS와 IMU에 칼만필터를 씌운다.
> 지하에서 GPS가 잘 닿지 않거나, noise가 있을 수 있다.

2. 카메라를 이용해 stereo odometry <br>
keyward : <mark>disparty map{시차 맵}, trigonometry{삼각 측량법}, odometry{주행기록}, stereo vs monocular{카메라를 한대 사용 할 것인가? 두 대 사용할 것인가?}</mark> <br>
> 내가 고등학교 때, 고민했던 분야이다! <br>
> 카메라 두개로, 두 카메라 영상의 차이를 이용해서 물체간의 거리를 알아낸다.<br>
> 어두운 터널같은 곳에서 인식하기 힘들 수 있다.<br>

3. Lidar를 활용한 localization <br>
keyward : <mark>particle filter, point cloud shape description</mark>  <br>
> lidar에서 매우 많은 point가 존재하기 때문에, 포인트 단위로 관측하는 것은 효과적이지 않다.<br>
> 파티클 필터를 통해 기존에 파악된 맵과 기존의 맵을 비교하는 방식이 필요하다.<br>
> 비가 오거나 공기중의 부유 입자가 많으면 노이즈가 커진다.

##### 1-2-2) object detection, object tracking
keyward : <mark>CNN, trajectory{궤도}, Auto-Encoder</mark>  <br>
컴퓨터비전 deep learning이 사용되는 분야이다.<br>
object detection을 위해서는 CNN이 주로 이용된다. {CNN은 지금 공부 중인 분야라 나중에 적겠다.}<br>
object tracking을 위해서는 보행자의 trajectory를 estimate 해야 한다.<br>
auto encoder라는 딥러닝의 비지도학습 분야를 사용한다. 보조 이미지를 활용한 auto encoder<br>
다양한 시점의 차량의 위치변화에 안정적으로 대처하도록 사용한다고 한다.<br>

##### <mark> 필자 : 아마 모터에서 주는 odometry도 활용할 것이다. </mark>

<br>

#### 1-3) decision making
##### 1-3-1) action prediction
다른 물체가 어디로 갈지 예측한다.<br>
ex)다른 차량이 특정 지점으로 가는 확률 분포를 만든다. <br>

##### 1-3-2) path plan
실시간으로 어떤 길을 가야 할지 결정한다.<br>
확률기반으로, cost function이 가장 적은 곳으로 경로를 결정한다.<br>
시간복잡도가 적어야한다. <br>

##### 1-3-3) obstacle avoidance 
두 단계 이상에 거쳐서 방지한다.<br>
능동형과 수동형으로 나눈다면, <br>
능동적 : traffic prediction을 한다. 충돌까지 남은 시간이나 최소 거리를 추정한다. <br> 
수동적 : 갑자기 rader에 물체가 검출되면 멈춘다. <br>

<br>

### 2) client system. (HW, OS) <br>
keyward : <mark>robust{견고}, realtime, reliable, ROS</mark>  <br>
신뢰성과 실시간성이 가장 중요하다. <br>
견고하게, 프로그램이 죽어도 다시 살아나야한다. <br>

#### 2-1) ROS
robot operating system. 아직 3가지 부족한점이 있다.<br>
- 안정성{reliablity, stability, robust}
- 성능{Perfomance} broadcasting
- 보안{security}

- 안정성 <br>
ros는 ros master가 죽으면, system 전체가 멈춘다. <br>
아파치 zookeeper : 분산 시스템을 관리하기 위해 사용하는 프로그램.<br>
분산 처리 시스템(Distributed Processing System) : 운영체제 강의에서 배운다. <br>
docker에 kubernetics 와 비슷한 느낌인가? <br>
jenkins는 무엇인가? <br>
<br >
- 성능 <br>
ROS 통신 loop-back 매커니즘을 통해 통신 : <br>
broad casting vs multi casting <br>

##### <mark> 필자 : linux의 자율주행용 OS AGL이 있다. </mark>


### 3) server computer

simulation
