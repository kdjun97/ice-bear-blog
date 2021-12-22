---
layout: post
title:  "Spring MVC Project"
date:   2021-12-20 14:40:30 +0800
categories: [Tech]
excerpt: 스프링 프레임 워크 개발
tags:
  - Project
  - Web
  - Java
  - Spring
---

# 스프링 프레임 워크 개발(Web)  

학기 중, 간단한 웹 서비스를 만드는 프로젝트가 있었다.  
기간은 사실상 거의 2주?도 안줬던 것 같다.  

각설하고, 바로 본론으로 들어가보자.  

**Info**  
- Version : 1.0.0  
- Development Environment
  - STS 4.11.1  
  - jre 1.8  
- Deploy URL: [Deploy Link Here](https://angel10004.herokuapp.com/)

**Description**  
카풀 이용자와 카풀 운전자끼리 연결을 맺어주고, 입금 관리까지 도와주는 서비스이다.  
이 서비스에는 특이한 기능이 있는데, 그 기능은 black 기능이다.  
입금을 하지 않았던 사용자의 black값이 count되고, 그 count는 방에서 값이 보여지게끔 되어있다.  
따라서, 이용자로 하여금 경각심을 가지게 하며, 서로 거래를 깔끔하게 하자는 취지로 만든 간단한 웹이다.(사실은 카풀 운전자들의 의견을 수렴하여 만든 웹이기도 하다.)  

**Usage**  
회원가입을 하고, session 정보가 있는 회원만 들어갈 수 있다.  
또한, room 하나에는 이용자 한 명만 들어갈 수 있다.  
방장만이 입금 여부와 추방을 할 수 있는 권한이 있고, 방을 만들 때, contents를 넣을 수 있게 하여 메시지를 표시하게 했다.  
운행을 마친 카풀 운전자(방장)가 방을 파기할 수 있다.  
이렇게 간이로 카풀 운전자와 카풀 이용자끼리 방을 만들어 매칭을 시켜줄 수 있고, 인원은 최대 4명으로 제한되어있다.  

**DB Schema**
디비 이미지 삽입

**History**
```
fdfd
dslkfjf
dflskdjf
fldkfjdslfkj
dflkdjfld
fldkfjd
fdjlfkdjf

```

---  

#### 프로젝트 생성에 앞서

스프링 framework는 웹을 개발할 때, 많이 사용되는 framework이다.  
그 이유로는, 잘 설계된 웹 MVC framework가 있고, 모듈화가 잘 되어있다는 점 또한 꼽을 수 있다.  
그리고, JDBC 등 기술을 위한 다양한 템플릿을 제공해준다.  
결과적으로, **Java를 위한 가장 인기있는 애플리케이션 개발 프레임워크**이기 때문에, Spring framework로 개발을 진행할 것이다.  

#### DB

DB는 학교에서 제공해준 phpMyAdmin 서버를 이용하였다.  
Tomcat  

**Database 구상**  
- ㅁㄴㅇㄹ  


#### 진짜 프로젝트 시작

1. STS4(Spring 3 Add-on 설치)  
  - Help -> Eclipse Marketplace에서 설치.  
2. 새 프로젝트 생성  
  - File -> New -> Spring Legacy Project -> Spring MVC Project  
  ![MVC](/assets/images/Spring_MVC/mvc.jpg)  
