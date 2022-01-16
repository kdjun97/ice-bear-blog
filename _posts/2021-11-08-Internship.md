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
실습내용 : 본사 서비스용 어플 관리 및 개선 / 신규 사업 프로토타입 업무  
근무부서 : 사업팀 개발부부  
담당업무 : 어플리케이션 관리 및 개선  

--- 

> 지금은 AutoML이 Google Vertex AI 쪽으로 넘어가거나 흡수된 것 같음. 확인해 볼 필요 있음. (정확하지 않음)  

> 포스팅 관련해서 회사에 물어본 결과, 특정 회사이름이나 서비스 이름 등이 노출만 안되는 선에서 공부하고 아카이브하는 것은 가능하다고 하셔서 허락을 맡았다!  

### 개발했던 기능

``Text_Recognition``  
  - GCP(Google Cloud Platform)에서 Cloud Vision사용.  
  - API 호출을 통해 Text_Recognition(OCR) option 사용. ROI Text추출, 추출한 value db에 올리는 과정을 담당.  
  - 이 기능을 이용하여, 건강검진 데이터에서 원하는 region에 있는 값들을 추출하여 db에 올릴 수 있음.  
  - 추후 db에 들어간 이 값들은 deep learning을 통하여, 향후 건강에 대한 예측할 때 사용됨.  
  - 혹은, 사용자에게 5년 혹은 10년간의 건강검진 데이터에 대한 값들을 그래프 혹은 직관적인 데이터로 보여줄 때 사용될 수 있음.  
  - 이 기능 역시, 실제로 서비스하는 앱에서의 기능 보다는, 테스트 프로젝트를 파서 기능이 잘 작동하는지를 확인하는 것에 초점을 두었다.  


**Flow**  

Goal : image -> Text -> ROI에서 수치 추출  

1. `User가 camera로 사진을 찍음.`
  * `edge_detection` plugin 사용, edge 추출을 기반으로 rect에 4개의 point를 추출.  
  * 추출된 point로 perspective transform을 수행.  
    - ![perspective transform1](/assets/images/HEM/ocr/perspective_transform.PNG)  [출처 : OpenCV Perspective Transformation](https://miatistory.tistory.com/5)  
    - ![perspective transform2](/assets/images/HEM/ocr/perspective_transform2.PNG)  
    - 위 그림을 참고하면 `perspective transform`을 쉽게 이해 가능할 것이다.  이 과정은 input image를 보다 정확하게 인식하기 위해 꼭 필요한 기능임.  
  * ![edge detection](/assets/images/HEM/ocr/edge_detection.jpg)  
    - 또한, 위 사진과 같이 각 point를 늘리거나 줄일 수 있음(이동 가능)
    - 정밀한 조정이 가능.
    - 이 4개의 point로 perspective trasnform 수행.  
  * **pre_processing된 Image를 저장.**  
    
2. `Text Recognition`
  * ``1번`` 과정에서 저장했던 img를 input으로 받고 output으로 text를 추출해 String으로 뽑음.  
  * Google Cloud Vision API를 사용(`TEXT_RECOGNITION`)  [출처 : Cloud Vision API](https://cloud.google.com/vision/docs/ocr)  
  * ![API_raw_result](/assets/images/HEM/ocr/API_raw_result.PNG)  
  * 위와 같이 하나의 String으로 API호출 결과를 받을 수 있다.
    
3. `Parsing Data 1`  
  * 2번을 일단 customized model에 넣어서 1차적으로 data를 정리한다.  
    - ![result_paper_model](/assets/images/HEM/ocr/result_paper_model.PNG)
    - 위 그림과 같고, List<model>으로 넣어주었다. 각 index에는 text description과 아래 그림과 같이 4개의 point가 있다.
    - ![point](/assets/images/HEM/ocr/point.PNG)
    - ![rect_sample](/assets/images/HEM/ocr/rect_sample.PNG)  
    
4. `Parsing Data 2`
  * p1이라는 절대적인 point를 기준으로 삼고, p1의 x좌표를 기준으로 sort를 함.  
  * List의 모든 요소들이 x좌표를 기준으로 sorting됨.  
  * row 1줄을 인식하여 그곳에서 원하는 데이터를 뽑음.(아래 빨간줄에 걸리는 모든 text들을 동일한 row라고 판단)
  * ![row](/assets/images/HEM/ocr/row.jpg)  
    - 위 그림과 같이 '공복'이라는 text를 인식하고, '공복'에 해당하는 point를 알아낸다. 그 point들의 y좌표 근처에 있는 요소들만 뽑아내면 row 1줄을 얻어낼 수 있다.  
    - 위 그림을 예시로 들자면, '당뇨병', '공복', '혈당', '(' 'mg', '/' 'dL', ')', '54', '100.0', '미만'... 등이 나올 것이다.  
    - 여기서 숫자에 해당하는 데이터만 뽑아 내가 원하는 수치를 뽑을 수 있다.  
    - 또한 위에서 <u>x좌표로 sort</u>를 해준 이유가 여기 있는데, x좌표로 sorting을 하고 나면, 처음 숫자가 나온 index에 내가 원하는 공복혈당 수치인 `54`를 뽑을 수 있게 된다.
    - 이런 방식으로 모든 원하는 수치들을 row 1줄 기준으로 뽑는다.  
  * user model이라는 새로운 모델에 parsing 완료된 값들을 넣는다.  
  * input image sample:  
    - ![input_img_sample](/assets/images/HEM/ocr/input_sample.jpg)  
  * output model sample:  
    - ![parsing complete](/assets/images/HEM/ocr/result.PNG)  
    - bmi는 몸무게/키(m)^2 공식으로 저체중, 정상, 과체중, 비만임을 식별한다(계산식을 사용해서 식별)
    
5. `다양한 인풋 이미지들로 성능 분석`
  * 다양하진 않고 8장의 사진들로 성능을 테스트 해보았다.
    - ![test_img](/assets/images/HEM/ocr/test_img.jpg)  
  * 한 두개씩 data를 못뽑는 경우가 간혹 있었다.  
    - 첫번째로 `isDigit`함수로 row 1줄에서 숫자를 뽑는 부분에서, 예를들면 키 '180.0/' 이런식으로 '/'가 같이 인식이 되어 값이 제대로 들어가지 않는 현상이 발견됐다.  
    따라서 '/'가 포함되었다면, '/'를 지우고 난 뒤, isDigit함수에 text를 넣는 방식으로 코드를 바꾸었다.  
    - 테스트 샘플에서 y값으로 인해 row에서 data하나가 빠지는 현상을 발견했다.  
    기존에는 1/2값으로 row를 판단하였지만, 지금은 1/5값과 4/5값으로 row를 판단하게 바꾸었다.  
    ![row](/assets/images/HEM/ocr/row_change.png)  
  * 하나씩 모델을 적용시켜보면서 case를 추가시키고 조건문을 바꾸며 최종적인 코드로는 8장 모두 정확도 100%로 인식했다.
    - ![test_img_result](/assets/images/HEM/ocr/test_img1.jpg)
    - ![test_img_result](/assets/images/HEM/ocr/test_img2.jpg)
    - ![test_img_result](/assets/images/HEM/ocr/test_img3.jpg)
    - ![test_img_result](/assets/images/HEM/ocr/test_img4.jpg)
    - ![test_img_result](/assets/images/HEM/ocr/test_img5.jpg)
    - ![test_img_result](/assets/images/HEM/ocr/test_img6.jpg)
    - ![test_img_result](/assets/images/HEM/ocr/test_img7.jpg)
    - ![test_img_result](/assets/images/HEM/ocr/test_img8.jpg)
    
6. 앱에 적용하기.  
  * 기능 확인 했으니, 응용하여 앱에 적용하면 된다.  

---  

``AutoML Vision``  
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

---  

``AutoML Labeling Web``  
