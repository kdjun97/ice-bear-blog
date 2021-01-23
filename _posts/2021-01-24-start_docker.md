---
layout: post
title:  "도커(Docker) 입문"
date:   2021-01-24 01:40:30
categories: [Tech]
excerpt: Let's start Docker!
tags:
  - Docker
  - Dockerfile
---
> 참고: GHOST GBC 1주차에 Programmer Base를 통해 도커에 대해 알려주신 한찬솔님께 감사드립니다.  

> 출처: [GBC_Programmer_Base](https://ccss17.github.io/ProgrammerBase/docker/#_1)  
정리에 앞서, 도커에 대한 설명이 위 출처에 상세하게 기록되어 있어 더 깊게 도커를 알아보고 싶으시면 위 출처를 참고하시기 바랍니다.  

---  

# 도커(Docker)에 대한 설명 

도커(Docker)가 무엇인가?  
- 도커(Docker)는 컨테이너 기반의 오픈소스 가상화 플랫폼이다.  
ex) 이 도커를 통해 Windows에서 Ubuntu Linux를 사용할 수 있다.  
- 내 Local에 굳이 Python, Linux, NodeJS같은 환경들을 설치하지 않아도, docker container에서 사용가능하다는 장점이 있다.  
- 또, 위험한 os실험을 하다가, 잘못되면 컨테이너만 날리면 되므로 아주 쓸만하다.  
- 도커는 운영체제 자체를 가상화 시켜 사용할 수 있게 해주었다.  
- 특징은 VMWare나 VirutalBox와 달리, 시스템 리소스를 딱 필요한 만큼만 사용하여 훨씬 가볍다는 것.  

![docker_image](/assets/images/docker/docker_image.PNG)  
도커는 위 사진과 같이,  
1. Dockerfile이라는 파일에 환경, 패키지들과 각종 셋팅(?)들을 작성해주고  
2. build를 해주어 Docker image를 생성한다.  
3. 이 image를 run하면 우리는 Dockerfile에서 설정한 환경의 Docker Container를 사용할 수 있게 된다.  

예시를 들어보자.  
만약, Dockerfile에 Ubuntu Linux에 대한 환경설정들이 되어있고, 그것을 build해서 Docker Image로 만들었다.  
그렇다면, run을 통해 우리는 Docker Container를 생성할 수 있게 되고, 이 Docker Container는 Ubuntu Linux에 대한 환경이 설정되어있다.  
즉, Ubuntu Linux의 환경 담은 가상환경이 만들어지는 것이다.  

> 이 기술로 Window에서도 Ubuntu Linux를 사용할 수 있다!  

---  
  
# 도커(Docker) 설치  

---  

#### MacOS 환경에서 설치하기  

[Docker_Mac Install](https://hub.docker.com/editions/community/docker-ce-desktop-mac)  

#### Windows 환경에서 설치하기
아쉽게도, MAC과 달리, Windows OS는 설치가 조금 까다롭다.  

1. 본인의 컴퓨터에 윈도우 버전 확인.  
- 방법 : 내 컴퓨터(우클릭) - 속성 - Windows 버전 확인.  

2. Windows 10 Home Edition이라면 업그레이드.    
- Home Edition을 사용중이라면, 에러가 뜰 수 있습니다.  
- Education으로 본인의 PC를 업그레이드 하셔야 Docker 사용에 문제가 없습니다.  
- Pro도 가능합니다.  

3. [Docker_Windows Install](https://hub.docker.com/editions/community/docker-ce-desktop-windows)에서 다운로드 받기  

4. Hyper-V enable → [BIOS] Intel - CPU Virtualization enable / AMD - SVM mode enable  
- 이 부분은 내가 MSI 노트북을 썻을 때, 따로 셋팅하지 않았다.  
이것은 노트북마다 다른 것 같다.  

---  

#### Docker에서 자주 쓰이는 명령어  

1. 컨테이너 이미지, 상태 확인  
- 컨테이너 상태 확인  
```$ docker ps -a : Docker 컨테이너 목록 출력```  
a옵션은 종료된 컨테이너까지 보여줌.  
- 컨테이너 이미지 확인  
```$ docker images : 로컬에 다운된 도커 이미지 목록을 보여줌```  

2. 컨테이너 실행  
- ```$ docker run --name mine -it kdjun97/ubuntu```  
-it은 터미널로 컨테이너에 접속할 수 있게 해주는 옵션  
--name은 컨테이너에 naming을 할 수 있게 해주는 옵션  
참고로 컨테이너를 종료하려면 ```$ exit```로 종료하면 된다.  

3. 종료된 컨테이너 재실행  
- ```$ docker start -ai 35ce92c```  
-ai는 컨테이너에 접속하여 터미널처럼 사용할 수 있게 해주는 옵션  
컨테이너 아이디가 `35ce92c`라면 ```$ docker start -ai 3```로 실행도 가능하다.  
하지만 컨테이너 아이디가 `35ce92c`, `3d921a2` 이렇게 두개라면, 35까지 입력해서 구분을 시켜줘야한다.  

4. 컨테이너,이미지 삭제  
- ```$ docker rm 35ce92c```  
컨테이너는 삭제해도 컨테이너 하나만 삭제된다.  
컨테이너에 쓰였던 이미지는 남아있어, 새로운 컨테이너를 다시 생성해서 다시 작업을 할 수 있다.    
- ```$ docker rmi kdjun97/ubuntu```  
(kdjun97/ubuntu 이미지 삭제)  
하지만, 이미지를 삭제하면, 다시 다운받아야한다.  

일단, 이정도만 알아도 docker를 요긴하게 사용할 수 있다.  

출처: [Ghost 한찬솔님의 블로그](https://ccss17.github.io/ProgrammerBase/docker/#_1)  

---  

#### Dockerfile build    

아까 Dockerfile을 build해서, docker image를 만든다고 하였다.  
docker image를 만든 소스코드가 Dockerfile이다.
Dockerfile을 작성했다면, docker build 명령어로 docker image를 생성 가능하다.  
- ```docker build --tag kdjun97/ubuntu```  
현재 디렉토리에 Dockerfile을 찾아, kdjun97/ubuntu라는 태그로 docker image를 생성함.  

> 나의 Ubuntu Unminimize Dockerfile을 참고하려면, [Dockerfile](https://github.com/kdjun97/ubuntu-unminimize/blob/master/Dockerfile) 참고!  

---  

#### Docker hub에 image 공유  
1. 먼저, ``` $docker login```명령어로 docker에 login한다.  
2. 이미지 태그  
``` $ docker tag kdjun97/ubuntu kdjun97/ubuntu```  
기존에 만든 kdjun97/ubuntu이미지에 kdjun97/ubuntu 태그를 추가하는 예(나는 똑같은 이름으로 했다.)  
앞에 나온 kdjun97/ubuntu가 기존에 build한 이미지고 뒤에는 rename을 해도 무방하다.  
보통, 버전도 붙여서 ```$ docker tag kdjun97/ubuntu kdj/ubun:1```이런식으로 적기도 한다.  
3. Docker gub에 image 전송  
``` $ docker push kdjun97/ubuntu```  
rename 한 예제,  
``` $ docker push kdj/ubun:1```  

---  

하지만, 매번 번거롭게 Dockerfile을 만들지 않아도 된다.  
이미, 누군가가 도커 허브에 Docker Image를 만들어놓았기 때문!  
도커 허브에서 다른 사람들이 등록한 도커 이미지를 찾아보고 사용도 가능하다.  
도커 허브에는 **Java, Python, gcc, MySQL, Ubuntu Linux, NodeJS** 등등 기업과 커뮤니티 혹은 개인들이 자신의 프로그램을 도커 이미지로 많이 만들어 놓았다.  
여기에서 본인이 편한 이미지를 찾아 사용해도 무방하다.  
관심이 있다면, 직접 찾아보는 것을 추천한다.  
[도커 허브 사이트](https://hub.docker.com/)  

---