---
layout: post
title: "사지방 개발환경 세팅하면서 나온 네트워크 용어"
categories:
  - Computer Network
tags:

last_modified_at: 2023-06-17
comments: true
---

하다가 모르는 용어들을 정리하고 있다. 

도커의 경우
dhcp
dns
[NAT 이해 blog](https://www.stevenjlee.net/2020/07/11/%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-nat-network-address-translation-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%A3%BC%EC%86%8C-%EB%B3%80%ED%99%98/)
[NAT and PAT](https://velog.io/%40yange/NAT%EC%99%80-PAT%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC)

### 2. NAT
NAT : Network Address Translation   
공유기 == 라우터 == 게이트웨이 
ipv6의 경우 ip의 수가 무한에 가깝고, end to end 연결을 이용하기 때문에 NAT이 필요하지 않다.  
보안과 ip주소 절약이 가능하다.  

PAT$($NAPT$)$ : 포트를 설정하여 패킷을 구분함.
같은 공유기에서 나온 서로다른 기기가 같은 ip로 접근한다면, 포트로 구분 해야 함. 

ipv4
ipv6

주소 할당 방식에 따른 NAT 분류
StaticNAT : external ip와 internal ip가 1대1매칭. 
하나의 서버가 여러 역할을 할 때, 포트포워딩 목적

DynamicNAT : external ip가 internal ip보다 많은 경우 

PAT$($NAPT$)$ : external ip 1개에 internal ip 여러개가 매칭.  
internal host에 포트 번호를 지정하여 해당 포트 번호를 

internal ip 
external ip
```
ifconfig:리눅스용 vs ipconfig: window용
```

ip의 열려있는 port 확인하는 법
```
netstat -tnlp
nmap www.google.com
nmap 0.0.0.0
```
결과
```
Host is up (0.00055s latency).
Not shown: 993 filtered ports
PORT      STATE  SERVICE
22/tcp    open   ssh
80/tcp    closed http
443/tcp   closed https
3389/tcp  closed ms-wbt-server
8000/tcp  open   http-alt
8080/tcp  closed http-proxy
12000/tcp closed cce4x
```

외부 아이피 확인
```
curl ifconfig.me
```
내부 아이피 확인 
```
ifconfig
```


---
포트 포워딩 예시 
linux에서는 1000번 때 이하의 포트는 관리자 권한이 있어야 binding 할 수 있다.  
root가 아닌 user 계정으로 포트 포워딩을 하면 다음과 같이 가능하다. 

포트 열기  
```
iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080
```
포트 닫기
```
iptables -t nat -D PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080
```
```
iptables -t nat -L
```
---


### 3. subnet mask

### 4. 라우터 게이트웨이 모뎀

### 5. 프록시

### Q. 다른 포트로 webserver를 구동시킬 수 있는가?

### rest api vs restfull api vs grapjql vs gRPC 

### cloud fare