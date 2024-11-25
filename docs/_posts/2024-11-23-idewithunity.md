---
layout: post
title: 맥북 에어 에서 유니티 개발 가능?
subtitle: IDE 찾기부터 유니티 개발 가능 한지 찾아보기
author: dduckchul
tags : [맥, 유니티, 일기, 메타버스부트캠프]
---

# 갖고 있는 장비에서 시작하기  
주인장은 회사에서 맥을 쓴 이후로 애플 실리콘에 대한 호감도가 매우 올라갔던바,  
실제 공부용 + 약간의 개발용으로 맥북 에어를 구매했었다 (작년말쯤..?)
 * 애플 실리콘 M2 CPU 8코어, GPU 10코어 16GB, 512GB SSD의 스펙

# C# 프로그래밍을 Mac OS에서?
결론만 이야기하자면 충분히 가능,  
마이크로소프트가 오픈소스 쪽으로 방향을 튼 이후로 맥 OS의 C# 지원을 잘해준다.  

IDE는 물론 윈도우 프로그래밍에서 자주 사용하는 Visual Studio는 쓰지 못하지만,  
  * [맥용 비쥬얼 스튜디오 2022 버전 지원 종료](https://learn.microsoft.com/en-us/visualstudio/releases/2022/what-happened-to-vs-for-mac)  

Visual Studio Code의 C# 플러그인으로 어느정도는 가능.  
  * [VS code](https://code.visualstudio.com)

혹은 JetBrains의 Rider라는 상용 프로그램도 쓸만하다고 한다.
  * [Rider](https://www.jetbrains.com/ko-kr/rider/)

닷넷 프레임 워크의 맥 OS 지원 여부는 아래 문서를 참고하면 좋을듯!
  * [MacOS의.net지원](https://learn.microsoft.com/ko-kr/dotnet/core/install/macos)

## 난 뭘 고를것인가?
  * 기존에 살짝 살짝 코딩하는 용으로 비쥬얼 스튜디오 코드를 사용했으나,  
  자바 시절 개발했을때 IntelliJ를 잘 사용했던 경험으로, Rider를 써보는것으로 결정!
  * 또한 개인용, 교육용으로는 무료인점도 매력적이다.
  

# 맥에서도 게임 프로그래밍 할수 있어?
몇번 친구에게 물어보았다. 맥에서도 유니티 프로그래밍은 충분히 가능.  
벤치마크를 몇개 보아도 프로는 충분히 가능할 것 같다.
  * [노트북 벤치마크](https://nanoreview.net/en/laptop-compare)  

그러나 내가 갖고있는건 그렇게 퍼포먼스를 생각하지 않고 구매한 맥북 에어 😂  
다행히도 테스트한 유튜버가 있긴하다 (3년전 자료이지만..)

<iframe width="560" height="315" src="https://www.youtube.com/embed/L4NbHk5f2Bs?si=KzL8kLK-qJIzb94X" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
<iframe width="560" height="315" src="https://www.youtube.com/embed/DO39o5lQanQ?si=SShUd6HDSejLiUd1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

# 결론?
직접 윈도우 노트북과 비교한 벤치마크는 없긴하지만,  
M1 프로와 비교해서는 대략 30%~40% 정도 뒤쳐지는듯 하다.  
  * 역시 GPU 성능이 비교하기가 어려울듯.  
  * 쓰로틀링 까지 발생하는데 에어는 팬이 없어서 프로젝트가 커질수록 치명적일수도?  

걱정했던것처럼 아예 프로그래밍이 안되는 수준은 아닌듯.  
일단 학습용으로는.. 어느정도 해도 괜찮지 않을까..?  
동일한 프로젝트들을 학원 컴퓨터에서도 돌려보고 안되면 나중에 바꾸던가 해보자.

## 추가 첨부
* 학원 컴퓨터의 성능이 내장그래픽인걸 확인 (Ryzen 5600G) / 내꺼가 더 좋을거같은데..?
* 윈도우에서 해보는 학습용으로 빼고는 그냥 내껄로 하자~
* 기본 실습을 닷넷 코어로 하지 않고 닷넷 프레임 워크로 하고 싶을때!
  * 조금 더 찾아보니 맥에서도 닷넷 프레임워크로 개발할수 있는 프로젝트도 있다
    * [모노 프로젝트](https://www.mono-project.com)
  * 아마도 조금 다를수 있겠지만 일단 오늘 배운 실습코드도 잘 되니 아주아주 만족~~