---
layout: post
title:  "Internship"
date:   2021-11-08 21:00:00
categories: [Tech]
excerpt: 어플리케이션 개발  
tags:
  - Flutter
  - Web
  - Internship
---

### 2021 하계 인턴

기간 : 2021.06.21 ~ 2021.08.27  
회사명 : 주식회사홀잡펠이펙티브마이크로브스  
실습내용 : 본사 서비스용 어플 관리 및 개선 / 신규 사업 프로토타입 업무  
근무부서 : 사업팀 개발부부  
담당업무 : 어플리케이션 관리 및 개선  

--- 

> 지금은 AutoML이 Google Vertex AI 쪽으로 넘어가거나 흡수된 것 같음. 확인해 볼 필요 있음. (정확하지 않음)  

### 개발했던 기능

1. Text_Recognition  
- GCP(Google Cloud Platform)에서 Cloud Vision사용.  
- API 호출을 통해 Text_Recognition(OCR) option 사용.  
ROI Text추출, 추출한 value db에 올리는 과정을 담당.  
- 이 기능을 이용하여, 건강검진 데이터에서 원하는 region에 있는 값들을 추출하여 db에 올릴 수 있음.  
- 추후 db에 들어간 이 값들은 deep learning을 통하여, 향후 건강에 대한 예측할 때 사용됨.  
- 혹은, 사용자에게 5년 혹은 10년간의 건강검진 데이터에 대한 값들을 그래프 혹은 직관적인 데이터로 보여줄 때 사용될 수 있음.  

2. AutoML Vision
- AutoML Vision의 성능을 테스트하기 위해 진행된 테스트 프로젝트 진행  
- 따로 모델을 학습시키는 과정을 포함하지 않고, AutoML에서 제공해주는 Vision 모델로 자동으로 학습을 시킴.  
- 수동으로 사진을 올리고, 사진에 대해 label들을 달아줌.  
![result](/assets/images/HEM/labeling.png)  
- **Image Classification**기능을 이용하였고, AutoML에서는 자동으로 모델을 학습해주고, 학습된 모델을 **.tflite**형태로 뽑아줌.  
- 만들어진 **.tflite** 모델을 flutter 내의 `tflite` 플러그인에 있는 LoadModel 함수의 input parameter로 주게 되면, output이 나온다.  
![load](/assets/images/HEM/loadModel.png)  
- output은 분류된 label들의 좌표들에 대한 값이고 detectedClass명이 들어가있다.  
![xy](/assets/images/HEM/xy.png)  
- 이 class명과 점에 대한 값을 미리 만들어둔 model에 넣어, input image에 stack을 쌓아 보여주면 image label classification이 완성.  
- 결과  
![result](/assets/images/HEM/result.jpg)  
- 성능은 굉장히 좋다. 그리고, 따로 학습시킬 때, 활성화 함수, epoch 등 아무것도 고려하지 않고, labeling만 하면 되기 때문에 정말 간편하다.  
- 단점은 수동으로 labeling을 해주어야 한다는 점이다.  
- 본 테스트 프로젝트는 100장의 이미지로 학습을 진행하였지만, 실제로는 10만장 100만장 등 엄청난 갯수의 test data가 필요할 것이고, 검증 데이터도 필요할 것이다. 따라서, 사람 손으로 직접 labeling을 하기에는 무리가 있다고 판단하였다.  
- 후에, 이 수동 labeling 작업을 외주를 맡기고 그 관련 프로그램을 만들면 어떨까 라는 생각으로 3번째 프로젝트를 진행하게 된다.  

3. AutoML Labeling Web  
