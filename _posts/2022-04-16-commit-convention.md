---
layout: post
title:  "Git Commit Convention"
date:   2022-04-16 10:00:00 +0800
categories: [Tech]
excerpt: git commit convention (Feat. gitmessage.txt)
tags:
  - git  
  - github
---

### XXX Convention

xxx 컨벤션에 대해 들어본 적이 있는가?  
개발자들은 대표적으로 naming convention에 대해 먼저 떠올릴 것이다.~~아님말고ㅋ~~  
naming convention은 쉽게 말하자면 **이름 짓기 약속**이라고 보면 될 것 같다.  
예를 들어, 카멜 케이스(camelCase), 스네이크 케이스(snake_case), 파스칼 케이스(PascalCase) 등이 있다.  
눈치를 챘을수도 있는데, 나는 이번 포스팅에서 git commit convention에 대해 다룰 것이다.  

---  

### Git Commit Convention

그렇다면 커밋 컨벤션은 무엇일까?  
Commit Convention은 쉽게 commit할 때 commit message에 대한 약속이라 봐도 무방할 것 같다.  
최근에서야 커밋할 때도 commit convention이 있음을 알게 되었지만, 그 전에는 몰라서 못썼다.  
커밋 컨벤션 없이도 commit하는데에는 무리가 없다.  
하지만 왜 귀찮게시리 약속을 정해 놓은것일까?  

현업에서 협업을 할 때, 또는 본인이 알아보기 쉽게 등 많은 이유에서 커밋 컨벤션이 필요하다.~~(물론 내 생각이다)~~  
그럼 서두가 너무 긴데, 정확히 무엇인지에 대해 알아보자.  

### 커밋 컨벤션

커밋 메시지에 대한 약속.  
커밋 메시지 구조는 크게 3가지로 나뉜다(제목, 본문, 꼬리말)  
```  
type: Subject -> 제목  
(한칸 띄우기)  
body(생략 가능) -> 본문  
(한칸 띄우기)  
footer(생략 가능) -> 꼬리말  
```  
각 커밋 메시지 구조에는 규칙이 존재한다.  
아래에서 좋은 커밋 메시지를 만드는 규칙에 대해 언급하겠다.  

#### Commit Type

Type | 설명  
--------------|-------------------------
Feat:         | 새로운 기능 추가
Fix:          | 버그 수정 또는 typo
Refactor:     | 리팩토링
Design:       | CSS 등 사용자 UI 디자인 변경
Comment:      | 필요한 주석 추가 및 변경
Style:        | 코드 포맷팅, 세미콜론 누락, 코드 변경이 없는 경우
Test:         | 테스트(테스트 코드 추가, 수정, 삭제, 비즈니스 로직에 변경이 없는 경우)
Chore:        | 위에 걸리지 않는 기타 변경사항(빌드 스크립트 수정, assets image, 패키지 매니저 등)
Init:         | 프로젝트 초기 생성
Rename:       | 파일 혹은 폴더명 수정하거나 옮기는 경우
Remove:       | 파일을 삭제하는 작업만 수행하는 경우

#### Subject Rule

**1.** 제목은 최대 50글자 넘지 않기  
**2.** 마침표 및 특수기호 사용x  
**3.** 첫 글자 대문자, 명령문 사용  
**4.** 개조식 구문으로 작성(간결하고 요점적인 서술)  

#### Body Rule

**1.** 한 줄당 72자 내로 작성  
**2.** 최대한 상세히 작성  
**3.** 어떻게 보다는 '무엇을', '왜' 변경했는지에 대해 작성  

#### Footer Rule

**1.** `유형: #이슈 번호`의 형식으로 작성  
**2.** 이슈 트래커 ID를 작성  
**3.** 여러개의 이슈 번호는 ,로 구분  
**4.** 이슈 트래커 유형은 아래와 같다  

Issue Tracker | 설명  
--------------|-------------------------
Fixes:        | 이슈 수정중(아직 해결되지 않은 경우)  
Resolves:     | 이슈를 해결한 경우
Ref:          | 참조할 이슈가 있을 때 사용
Related to:   | 해당 커밋에 관련된 이슈 번호(아직 해결되지 않은 경우)

ex) Footer에 `Fixes: #1` 이라고 작성하고 commit을 할 경우, 자동으로 issue #1과 매칭이 된다.  
또한, `Resolves: #1`으로 이슈를 해결했다고 명시하면 그 이슈는 사라진다.~~한 번 써보면 감이 온다~~  


### 커밋 템플릿

아래는 현재 내가 쓰고있는 커밋 메시지 템플릿이다.  

```  
# 제목은 최대 50글자까지 아래에 작성: ex) Feat: Add OAuth2  

# 본문은 아래에 작성  

# 꼬릿말은 아래에 작성: ex) Github issue #23  

# --- COMMIT END ---  
#   <타입> 리스트  
#   feat        : 기능 (새로운 기능)  
#   fix         : 버그 (버그 수정)  
#   refactor    : 리팩토링  
#   design      : CSS 등 사용자 UI 디자인 변경  
#   comment     : 필요한 주석 추가 및 변경  
#   style       : 스타일 (코드 형식, 세미콜론 추가: 비즈니스 로직에 변경 없음)  
#   docs        : 문서 수정 (문서 추가, 수정, 삭제, README)  
#   test        : 테스트 (테스트 코드 추가, 수정, 삭제: 비즈니스 로직에 변경 없음)  
#   chore       : 기타 변경사항 (빌드 스크립트 수정, assets, 패키지 매니저 등)  
#   init        : 초기 생성  
#   rename      : 파일 혹은 폴더명을 수정하거나 옮기는 작업만 한 경우  
#   remove      : 파일을 삭제하는 작업만 수행한 경우  
# ------------------  
#   제목 첫 글자를 대문자로  
#   제목은 명령문으로  
#   제목 끝에 마침표(.) 금지  
#   제목과 본문을 한 줄 띄워 분리하기  
#   본문은 "어떻게" 보다 "무엇을", "왜"를 설명한다.  
#   본문에 여러줄의 메시지를 작성할 땐 "-"로 구분  
# ------------------  
#   <꼬리말>  
#   필수가 아닌 optioanl  
#   Fixes        :이슈 수정중 (아직 해결되지 않은 경우)  
#   Resolves     : 이슈 해결했을 때 사용  
#   Ref          : 참고할 이슈가 있을 때 사용  
#   Related to   : 해당 커밋에 관련된 이슈번호 (아직 해결되지 않은 경우)  
#   ex) Fixes: #47 Related to: #32, #21  
```  

### 커밋 예시

위 커밋 컨벤션을 참고하여 기능 추가에 대한 커밋 메시지를 남겨보자.  
우선, `$ git commit` 명령어로 커밋 템플릿에 들어가보자. 템플릿은 아래와 같이 생겼다.  
![commit](/assets/images/commit-convention/commit.JPG)    

위 템플릿에 공백 3줄이 우리가 채워넣을 line이다.  
signin, signup 기능이 추가되었다고 가정해보자.  
커밋 메시지는 아래와 같을 것이다.  

```  
Feat: Add signin, signup  
  
회원가입 기능, 로그인 기능 추가(예시를 위해 간단히 작성)  

Resolves: #1
```  
템플릿에는  
첫번째 줄엔 Feat: Add signin, signup  
두번째 줄엔 signup과 signin을 어디에 추가했고, 왜 추가했는지에 대한 글을 남긴다.(생략 가능)  
세번째 줄엔 issue에 대한 부분은 없지만 이슈1을 해결했다고 가정하자(생략 가능)  
wq로 저장하고 나가면 커밋이 자동으로 된다.(vim editor임)  
이런식으로 커밋을 하게 되면, 약속대로 signin, signup기능이 추가되었음을 메시지를 통해 직관적으로 알 수 있게 된다.  

### 템플릿은 어떻게 적용을 시키는가?
  
**1.** 본인이 원하는 경로에 `gitmessage.txt` 파일을 생성한다.  
**2.** 내용은 위 템플릿 내용을 넣는다.(#은 주석임)  
**3.** 터미널에 명령을 입력한다.  
`$ git config --global commit.template C:\.gitmessage.txt`  (예시를 위해 C:\ 경로를 주었고 실제로는 본인 경로에다가!)  

이 설정을 통해 앞으로 `$ git commit` 명령어를 통해 앞전에 만들었던 vim의 메시지가 보일 것이다.  
이제 본인만의 템플릿으로 커스터마이징 해서 간편하고 똑똑하게 커밋하자.  
  
**오류나 typo, 버그에 대한 지적은 환영입니다.**  