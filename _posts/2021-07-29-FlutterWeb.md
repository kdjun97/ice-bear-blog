---
layout: post
title:  "Flutter Web"
date:   2021-07-29 21:00:00
categories: [Tech]
excerpt: Flutter로 Web 개발 환경 구축
tags:
  - Flutter
  - Web
---

### Flutter로 Web 개발 환경을 구축해보자

[Flutter 공식 사이트](https://flutter-ko.dev/docs/get-started/web)

1. 평소대로 flutter project를 판다.
2. Flutter 내의 terminal에서 명령어 입력
```
$ flutter config --enable-web
$ flutter create .
$ flutter run -d chrome
$ flutter build web
```
3. Firebase가 연동되어 있다면, Firebase Console에서 셋팅을 해주어야 함.
  - Firebase Console에서 앱 추가 -> 플랫폼 선택-> Web
  - ![firebase1](/assets/images/flutter_web/firebase1.png) 
  - ![firebase2](/assets/images/flutter_web/firebase2.png) 
  - 이름을 설정하고 import해야하는 부분을 import해줌.
  - ![firebase3](/assets/images/flutter_web/firebase3.png) 
  - 자신의 flutter project 디렉토리 안의 web폴더에 index.html에 위와 같은 스크립트를 넣어주면 됨.  
  - firebase auth를 사용하려고 했는데, 에러를 직면했고, index.html에 다음이 필요하다는 것을 알게 됨.  
  ```{.html}
  <script src="https://www.gstatic.com/firebasejs/8.6.1/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/8.6.1/firebase-analytics.js"></script>
  <script src="https://www.gstatic.com/firebasejs/8.6.1/firebase-auth.js"></script>
  <script src="https://www.gstatic.com/firebasejs/8.6.1/firebase-firestore.js"></script>
  ```
  - 필요할 경우 terminal에서 **flutter build web** 으로 다시 빌드 하고 다시 chrome으로 실행
4. 결과  
![firebase4](/assets/images/flutter_web/firebase4.PNG)  

#### 이슈

Web에서 Image.network를 사용했을 때, path가 맞더라도 image를 read못하는 현상을 발견했다.  
이는 Terminal에서 $flutter run -d chrome --web-renderer html 명령어로 실행하니 해결됐다.  