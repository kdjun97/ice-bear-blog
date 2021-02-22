---
layout: post
title:  "Project 시작"
date:   2021-02-01 14:40:30 +0800
categories: [Tech]
excerpt: Using Flutter, I'm gonna make a program to communicate through lots of clients.
tags:
  - Project
  - Flutter
  - GHOST
  - GBC
---

괜찮으시겠어요? 아뇨 ㅋ 안괜찮음.    
Project Title: 승화는_거짓말쟁이 (Liar Game)  
Description: Flutter를 이용하여 Liar 게임 만들기  
2/1부터 시작.  
다소 정리가 안되어있는 부분은 프로젝트가 끝나고 정리해봄.  

## 네트워크가 통신하는 방법

인터넷 상에서는 서로 다른 기기(혹은 PC)끼리 통신을 할 때, 아이피가 필요하다.  
> IP: 컴퓨터 네트워크에서 장치들이 서로를 인식하고 통신을 하기 위해서 사용하는 특수한 번호  

출처: [위키백과](https://ko.wikipedia.org/wiki/IP_%EC%A3%BC%EC%86%8C)  

아이피는 다시 공인 아이피(Public IP)와, 사설 아이피(Private IP)로 나뉜다.  

`공인 IP(Public IP)`  
- 인터넷 사용자의 Local Network를 식별하기 위해 ISP(Internet Service Provider)가 제공하는 IP 주소.  
- 세상에서 단 하나뿐인 IP  
- 외부에도 공개되어 있기 때문에, 인터넷이 연결된 다른 PC로부터의 접근이 가능.  

`사설 IP(Private IP)`  
- 내부망 전용 IP  
- 일반 가정이나 회사에 할당된 네트워크의 IP  
- IPv4의 IP Address 부족 때문에 서브넷팅된 IP  
- 내부망에서만 사용되기 때문에, 서로 다른 내부망에서 동일한 IP를 구성하더라도 상관 없다.  
- 보통 192.168.x.x로 구성됨.  

`고정 IP`  
- 컴퓨터에 고정적으로 부여된 IP  
- 한번 부여되면 IP를 반납하기 전까지는 다른 장비에 할당되지 않음.  

`유동 IP`  
- 고정적이지 않은 IP  
- 컴퓨터를 사용할 때 남아있는 IP중 돌아가면서 부여  

![통신 이해](/assets/images/Project_Liar/Communication.png)  
위 그림을 보자. [출처 여기](https://velog.io/@hidaehyunlee/%EA%B3%B5%EC%9D%B8Public-%EC%82%AC%EC%84%A4Private-IP%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)  
사설망1에 있는 핸드폰은 사설망2에 있는 핸드폰과 통신을 할 수 없다.  
또, 인터넷에서는 공인 IP 178.145.57.144에 접속할 수 있지만, 사설IP인 192.168.x.x에는 접속을 할 수 없다.  
이렇게 사설 IP가 할당된 장비는 공유기를 통해 인터넷에 접속을 할 수 있게 된다.  
그리고 사설망1과 사설망2끼리의 통신도 이런식으로 외부로 다시 갔다가 내부로 들어와야한다.  

또한, 인터넷 상에서 서버를 운영하고자 할 때는 공인 IP를 고정 IP로 부여해야 한다.  
공인 IP -> 나만의 서버를 만들고,  
고정 IP -> 내 서버가 아닌 다른 사람의 IP로 접속될 수 있음을 방지  

현재 가정에서 사용하는 인터넷 서비스는 각 가정마다 공인 IP를 유동 IP로 부여, 공유기 내에서도 사설 IP를 유동 IP로 부여하고 있다.  

채팅(정보 통신)을 위한 서버는 대부분 고정 IP를 사용해야한다.  
하지만 고정 IP의 서버를 구축하려면 비용이 발생한다.  

본 프로젝트에서는 서버를 구축하는 비용을 없애기 위해 같은 공유기 내에서만 통신이 가능하도록 구현할 것이다.  
> 본 프로젝트에서는 외부와 통신이 필요없지만, 만약 외부에서 내부로 통신을 해야하는 경우에는 port forwarding을 해야한다.  

## c언어에서의 socket 통신  

지금 언급하는건 1:1 통신이 아닌, 서버를 1개로 두고 다수의 클라이언트가 통신하는 상황이다.  
아주 간략하게 적어보자면,  

1. socket을 생성(서버용)  
2. 각 구조체를 정의해줌(주소나 포트같은것들)  
3. bind  
4. listen  
5. while으로 루프를 돌면서 client의 connection 요청을 계속 accept해주며 다수의 클라이언트와 연결을 한다.(iterative 서버)  
6. 서버가 연결된 클라이언트들에게 broadcast해줌.  

이 방식을 적용했을 때의 문제점  
: 만약 3명이 게임을 하고싶다면, 4명이 필요함(1명은 서버를 열어주고 데이터를 처리해야 함. 이 때, 서버는 데이터를 뿌려주기만 하기에 아무 역할을 하지 않음.)  

## flutter(dart)의 socket 통신

flutter에서는 c언어에서의 socket programming과 다르게, 서버가 애초에 iterative하게 작동하는 것 같다.  
실제로, ServerSocket은 수신 소켓에 연결된 각 소켓에 하나씩 소켓 객체의 스트림을 제공한다고 한다. [출처](https://api.dart.dev/stable/2.10.5/dart-io/ServerSocket-class.html)  

1. fllutter는 ip와 port정보로 ServerSocket을 바로 bind한다.  
2. ServerSocket.listen(Function)만으로 listen과 accept과정을 다 수행해주는 것 같다.  

실제로 이 두 줄로, 다중 클라이언트들의 connection을 받을 수 있었다.(client가 connection을 요청하면 listen에 물린 함수가 실행됨. 또한, while로 accpet를 할 필요가 없었다.)  

client가 connection요청을 할 때, ServerSocket.listen(Function)의 파라미터인 Function함수가 실행이 되는데, 이 함수는 다시(Socket s)라는 파라미터를 전달하면서 실행이 된다.  
(즉, connection이 되면, Function이 실행되고, Function은 Socket s의 정보를 가지면서 실행됨)  
여기서 Socket s가 클라이언트의 정보다.  
즉, ServerSocket은 ```ServerSocket.listen(Function)```를 통해 Function(Socket s){} 함수를 실행함.  

그렇다면, 연결된 client의 정보들만 가지고 있다면, data값을 서버에서 보내는 것이 가능할 것이고,  
위에서 언급한 문제점은 서버가 broadcast를 해주면서 data를 동시에 처리하게끔만 만들면 될 것이다.  
-> List로 클라이언트 정보를 저장해줌.  

이제 해야할 일은 방만들기 기능, 자동으로 IP셋팅, 한글 지원정도이다.  
++ 클라이언트 하나가 disconnect 하면 전체가 통신이 안되는 에러가 있음.
-> 해결함.  

아이피를 자동으로 뽑는건 두 개의 패키지를 사용해봄.  
- import 'package:get_ip/get_ip.dart';  
- import 'package:seeip_client/seeip_client.dart';  

근데 어떤 기기에는 IP를 잘 뽑는데, 어떤 기기는 아니었다.  
(실험환경: Android Galaxy Note9, S10+, S6, A6)  Note9랑 10+가 안뽑힘.  
현재는 수동으로 IP를 설정해주고 있고, 현재 통신은 잘 된다.  

데이터를 넘길 때 String을 조작을 해서, IP주소와 닉네임, 메세지를 함께 넘겨주고, 받을때는 다시 파싱을 하는 방법으로 세 가지 정보를 받을 수 있게 해준다.  

그리고 Route간에 data를 이동 가능하게 하여, 메인 화면에서 필요한 정보들을 다 받고, 실제 화면에서는 채팅창만 보이도록 설정할 것이다.  

마지막으로, 게임 알고리즘을 적용하여 라이어게임을 완성시키면 얼추 프로젝트가 끝날 것 같다.  