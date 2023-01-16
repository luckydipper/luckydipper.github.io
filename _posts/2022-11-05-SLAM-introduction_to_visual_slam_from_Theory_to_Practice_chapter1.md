---
layout: post
title: "Introduction to visual slam from Theory to Practice chapter1"
categories:
  - SlAM 
tags:
  - Visual odometry
  - Loop closure
  - Front end
  - Backend
  - Map
last_modified_at: 2022-11-05
comments: true
---

이 책에서 다루는 카메라는 intrusive$($주변 환경에 제약을 둬야하는$)$하지 않는 센서이다. 

## visual slam framework
![SLAM_framework](/assets/img/SLAM/22_11_05_intro_visual_slam1/slam_frame_work.png)

## Front end
Visual slam에서 slam의 frontend는 visual odometry이다.  
robot의 motion model과 perception model을 정의해서 만든다.  
여기서 feature를 추출하면 feature based slam이 된다.  
추출하지 않을 시 struct base slam이 된다.

## Backend
Backend는 frontend에서 나온 에러를 줄이는 방향으로 optimize한다.  
이때 loop closure도 같이 쓰인다.  

## Map
- Metric maps  
정확한 위치를 알아내기 위해 쓴다.  
  - sparse map : feature based map 특정한 landmark만 기억함.
  - dense map $($grid map$)$ 모두 grid에 넣음.

- Topological$($위상수학$)$ maps
  - relation amaong map element
![grid_vs_topology](/assets/img/SLAM/22_11_05_intro_visual_slam1/grid_map_vs_topology_map.png)

## 출처 
https://opensource.com/article/20/4/linux-binary-analysis  
https://www.researchgate.net/figure/Semantic-Grid-Map-and-Topological-Graph-generation-The-metric-map-is-enriched-with_fig6_290480748