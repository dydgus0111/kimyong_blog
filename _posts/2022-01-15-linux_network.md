---
title:  "[Linux] 리눅스 네트워크?"
excerpt: "[Linux] 리눅스 네트워크?"

categories:
  - Linux
tags:
  - [Linux, Network]

toc: true
toc_sticky: true
 
date: 2022-01-15
last_modified_at: 2022-01-15
---


### 네트워크 인터페이스 다루기

- 네트워크 인터페이스 정보 확인 및 수정 명령어 : ifconfig
    - ifconfig [인터페이스] [옵션]
    - ifconfig [인터페이스] up : 네트워크 인터페이스 작동
    - ifconfig [인터페이스] down : 네트워크 인터페이스 중지
    - ifconfig [인터페이스] [ip주소] up : ip주소 변경하여 인터페이스 작동(재부팅 시 다시 변경됨)
- lo : 루프백 인터페이스
    - 자기 자신과 통신 하는데 사용하는 가상 장치
- eth0 : 유선 네트워크 인터페이스. 랜카드
- wlan0 : 무선 네트워크 인터페이스
- 인터페이스 필드
    
    
    | HWaddr | 네트워크 인터페이스의 하드웨어 주소(MAC Address) |
    | --- | --- |
    | inetaddr | 네트워크 인터페이스에 할당된 IP 주소 |
    | Bcast | 브로드캐스트 주소 |
    | Mask | 넷마스크 |
    | MTU | 네트워크 최대 전송 단위(Maxium Transfer Unit) |
    | RXpackets | 받은 패킷 정보 |
    | TXpackets | 보낸 패킷 정보 |
    | collisions | 충돌된 패킷 수 |
    | Interrupt | 네트워크 인터페이스가 사용하는 인터럽트 번호 |
- ifconfig나 route를 이용해서 네트워크 정보 변경 시 재부팅 하면 정보가 원래대로 돌아오기 때문에, 영구적인 변경을 위해서는 네트워크 인터페이스 설정 파일(`/etc/network/interfaces`)을 수정해야함.
    
    ※Ubuntu에서는 **네트워크 매니저**라는 패키지가 네트워크 설정 중 우선순위가 가장 높음. 위의 파일을 수정하더라도 네트워크 매니저가 설치 되어 있다면 적용이 안될 수 있음.
    

### 라우팅 테이블 다루기

- 라우팅 : 네트워크를 통해 목적지로 패킷이 전송될 경로를 지정해주는 것을 라우팅 이라고 합니다.
- 명령어 : route
    - add : route 추가
    - del : route 삭제

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/896b581d-194f-421e-80dc-522b0bbcbff7/Untitled.png)

| Destination | 목적지 |
| --- | --- |
| Gateway | 외부 네트워크와 연결하기 위한 게이트웨이 주소 |
| Genmask | 목적지 네트워크의 넷마스크 주소.
0.0.0.0 : 기본 게이트웨이 주소
255.255.255.255 : 목적지의 호스트 주소를 의미 |
| Flags | 해당 경로에 대한 정보를 알려주는 기호
- U(Up) : 경로가 살아 있음
- H(Host) : 목적지가 호스트 주소
- G(Gateway) : 게이트웨이를 향하는 경로 |
| Metric | 목적지 네트워크까지의 거리 |
| Ref | 경로를 참조한 횟수 |
| Use | 경로를 탐색한 횟수 |
| Iface | 패킷이 오가는 데에 사용할 네트워크 인터페이스 |

### 연결 상태 진단 도구 ping

- 네트워크 연결 상태를 점검하는 명령
- 목적지에 ICMP(Ingernet Control Message Protocol) 패킷을 보내고 되돌아오는지 확인
- 명령어 : ping [옵션] 목적지주소(IP주소/dns주소)
    - ex) ping 8.8.8.8 , ping google.com

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb20f906-3c0e-46b7-9128-71a3a4e51dc2/Untitled.png)

### netstat으로 네트워크 정보 확인하기

- 리눅스 네트워크 상태를 종합적으로 보여주는 명령. 리눅스 서버의 열려있는 모든 소켓에 대한 정보를 확인 가능
- 명령어 : netstat
- 옵션

| i | 네트워크 인터페이스를 통해 주고 받은 패킷에 대한 정보 확인 가능 |
| --- | --- |
| nr | 라우팅 테이블 정보 확인 가능 |
| s | 프로토콜에 따른 패킷 통계 |
| atp | -열려있는 포트 번호
-데몬
-해당 포트를 사용하는 프로그램 정보 |
- State

| LISTEN | 서버의 데몬이 떠 있어서 클라이언트의 접속 요청을 기다리고 있는 상태 |
| --- | --- |
| ESTABLISHED | 서버와 클라이언트간 세션 연결이 성립되어 통신이 이루어지고 있는 상태 |
| TIME-WAIT | 연결은 종료되었지만 당분간 소켓을 열어놓은 상태 |
| SYN-SENT | 클라이언트가 서버에게 연결을 요청한 상태 |