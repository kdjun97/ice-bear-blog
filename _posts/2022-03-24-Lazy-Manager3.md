---
layout: post
title:  "Lazy Manager3"
date:   2022-03-24 16:00:00 +0800
categories: [Tech]
excerpt: Lazy Manager3
tags:
  - c#  
  - Project
  - Arduino
---

## Program Info

[Github Link](https://github.com/kdjun97/lazy-manager)  
수정중  

---  

## Stopped Project, Lazy Manager의 끝

이전 포스팅.~~참고하면 좋음~~  
[Lazy Manager](https://kdjun97.github.io/blog/Lazy_Manager/)  
[Lazy Manager2](https://kdjun97.github.io/blog/Lazy_Manager2/)  
생각만 했었고, 개발은 끝까지 진행하지 못했던 프로젝트이다.  
이제 이 프로젝트를 끝장낼 생각이다.  

---  

#### 기대효과

<details>
  <summary>기대효과</summary>  
  <p>
  사실 사람은 반복 작업을 하는 것을 아주 지루하게 느낀다.<br>
  나 역시 그렇게 느꼈었고, 그래서 이 프로그램을 생각하게 되었다.<br>
  예를 들어, 어떤 특정 버튼을 계속 눌러야할 때, 너무 귀찮지 않은가?<br>
  혹은 반복되는 키보드 입력으로 너무 힘들거나, 단 한번의 키 입력으로 100번의 키 입력 효과를 나타내는 편리한 기능을 생각해본 적이 있을 것이다.<br>
  보통 이런 경우는, 사무적인 업무에서 많이 발생된다.<br>
  따라서, 나는 이러한 귀찮은 업무를 대신해주는 혹은 더 편리한 기능을 제공해주는 프로그램을 만들어 인간이 더 편리하게 작업을 했으면 한다.<br>
  물론, 많은 사람들이 이러한 생각을 하고 도움을 주는 프로그램을 만들었을 것이다.<br>
  하지만 A회사에서 A프로그램을, B회사에서 B프로그램을 만드는 것 처럼, A회사 프로그램을 다른 회사에서는 사용할 수 없다.<br>
  이 프로그램은 사용자 정의 맵핑기능이 있어 더욱 많은 사용자들(기업들)이 입맛대로 바꾸어 사용할 수 있다는 장점이 있을 것이다.<br>
  또한, 활성창에서 이벤트 처리가 이루어지는 것이 아닌, 비활성창에서도 이벤트 처리를 할 수 있게 함으로써 사용자의 편의를 더욱 생각했다.(동시에 많은 일을 할 수 있음)<br>
  </p>
</details><br>

---  
  
#### TODO

**1.** 스크립트형 프로그램(사용자 정의 가능)  
  - base는 스크립트를 위한 메모장 클론 코딩
  - Script를 읽고 리스트로 관리
  - Script에 맞는 액션을 수행하는 코드 필요  

**2.** Arduino를 접목시킬 것.(hardware 신호 처리)  
  - Leonardo Board 사용, serial 통신 코드 짜야함  

**3.** 비활성 제어는 초기 버전에서는 고려 x

---   

#### 시작

**1.** load, save, new file 등의 메모장 기능들을 구현  

우선, 사용자가 스크립트를 직접 짤 수 있어야 한다.  
때문에, 메모장의 기능이 필요하다.  
우리가 익히 아는 `notepad.exe`와 비슷하게 winform을 구성한다.  
Winform 구성은 `MenuStrip`, `TextBox`, `StatusStrip`, `OpenFileDialog`, `SaveFileDialog`를 사용하였다.  
![UI](/assets/images/lazy_manager3/skeleton_ui.jpg)   

**2.** TextBox에 입력된 스크립트를 읽는 기능 필요  

TextBox에 있는 내용을 parsing을 하여 키코드, 명령어로 구분함.  
그 후, 모델 리스트에 넣어 리스트를 관리할 수 있어야 함.  
![Model_List](/assets/images/lazy_manager3/read_script_complete.JPG)  
> Script/ReadScript.cs에서 parsing과 hotkey를 setting해주는 기능을 담당한다.  

**3.** Hotkey 설정  

![Set Hotkey](/assets/images/lazy_manager3/set_hotkey.jpg)  
설정한 key에 hook을 걸어줌.  

**4.** 각 Hotkey에 command 설정  

![Set Command](/assets/images/lazy_manager3/set_command.JPG)   
각 명령에 대한 exception들이 많다. 처리해줘야한다.  
ex) m명령어랑 k명령어, s등이 섞여있을 때의 처리  

**5.** Command 처리  

각 핫키에 command들이 맵핑된 상태.  
이제 각 condition에 따라 keybd_event, mouse_event command를 처리해줄 차례  
condition은  `k`, `m`, `s`와 `q`같은 특수 명령어로 구분함.  
`k`: keybd_event  
`m`: mouse_event  
`s`: sleep(delay)  
`q`: quit(break 개념)  

> 기본 기능은 끝남. 추후 r(loop 기능) i?(ImageSearch)기능 등을 구현할 예정  

**6.** GUI  

Form을 유저가 사용하기 쉽게 직관적으로 디자인함.  
직관적인 main화면 외에 강력한 기능들은 `MenuStrip`에 배치.  
또한, 스크립트 작성 규칙을 만드는 중  



---  

**시행착오**  

`첫번째`  

`s` command 처리 중, delay관련 처리를 할 때, error가 났다.  
F1을 핫키로 설정했다면, 기존에는 s 명령어가 `Thread.Sleep(2000)`의 방식으로 처리 되었다.  
후킹도 잘 되었고, F1을 누를 때, 명령어 수행도 잘 되었지만, F1키가 잠기지 않았다.  
그 이유는 Thread.Sleep 자체가 현재 스레드를 멈추는 것이라서 후킹 관련 액션도 2초간 멈추었기 때문이라고 판단하였다.  
그래서 `s`명령어 처리를 `Task.Delay(int millisecond)`로 비동기식 처리를 하였다.  
이 명령어는 task를 지연시켜주는 명령어이기 때문에 s명령어 처리와 더욱 적합했고, 테스트 결과 키가 잠기는 것을 확인할 수 있었다.  

`두번째`  

mouse_event는 좌표값의 설정이 중요한데, 화면 해상도의 배율에 따라 좌표값이 달라지는 현상이 발견되었다.  
모든 user들의 해상도나 배율이 다를것이기 때문에 해상도 값을 처리할 방법을 생각중이다.  
다행히, stackoverflow에 display resolution에 관한 글이 있었고, 이를 참고하여 화면 해상도 배율을 곱하며 본래 해상도를 구할 수 있었다.  

[출처](https://stackoverflow.com/questions/5082610/get-and-set-screen-resolution)  