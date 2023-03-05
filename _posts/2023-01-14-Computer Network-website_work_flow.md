---
layout: post
title: "IP vs MAC vs PORT"
categories:
  - Computer Network
tags:

last_modified_at: 2023-0-05
comments: true
---
### port() vs ip address vs mac address : identifier$($식별자$)$

**MAC address** : L2로 NIC$($network interface card 또는 유무선 LAN 카드$)$식별자. 하지만 변경 가능.  

**IP address** : L3로 Host$($network에 접속한 컴퓨터$)$ NIC한개에 여러개의 IP bining 가능.  
프로세스 1개당 1개의 port번호를 갖을 수 있다.
IP 주소는 IANA RIS에서 각 국의 ISP로 ip를 나눠준다.  

**port** : L4로 Process or Service or Interface 식별자  
HW 딴에선 공유기 랜 케이블, L4에선 8080 80가 둘다  http service에 쓰인다.  
0~2^10까진 약속된 포트이다.

출처 :  
[널널 개발자](https://www.youtube.com/playlist?list=PLXvgR_grOs1BFH-TuqFsfHqbh-gpMbFoy)