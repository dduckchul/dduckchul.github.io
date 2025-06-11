---
layout: post
title: XR Origin과 유니티 스케일 맞추기
subtitle: VR의 고정 플레이모드 / 룸 스케일 모드에 따른 조작 변경해보기
author: dduckchul
tags : [유니티, VR, 메타버스부트캠프]
---
# 유니티의 기본 스케일은 X,Y 좌표 1당 1m로 환산 되어있다.
* XR Origin과 Tracked Pose Driver를 사용한다면, 현실과 유니티안의 움직임을 동일하게 맞출 수 있다.

| ![디바이스](/assets/img/posting/2025/VRScale/Tracked Pose Driver.png) | ![디바이스](/assets/img/posting/2025/VRScale/XR Origin.png) |
| ---------------------------- | ------------------ |
| Tracked Pose Driver          | XR Origin          |

* Position / Rotation Input을 XRI Head로, Tracking Type은 필요한 위치/로테이션 동기화 한다면
* HMD (헤드 마운트 디스플레이)의 움직임 (실제 사람의 움직임)에 따라 캐릭터를 움직이게 할 수 있다.

## XR Origin - Tracking Origin Mode
* 위 설정이 캐릭터의 움직임을 맞출때 매우 중요하다.
* 설정은 Not Specified / Device / Floor 로 나눠져 있다.
* Not Specified는 기기 설정에 따라 (라고 되어있는데 확실하지 않음)
* Device는 고정 플레이용으로 쓰기 적합하다.
* Floor는 Room Scale (현실 경계 그려서) 플레이용으로 쓰기 적합하다

### Tracking Origin Mode - Device
* 나의 현실 위치가 게임의 0,0,0위치로 변환 된다.
* 게임 시작 전에는 내가 어디 있든간에 유니티의 XR Origin 위치와 상관이 없다.
![디바이스](/assets/img/posting/2025/VRScale/Tracked Mode - Device.png)
---
### Tracking Origin Mode - Floor
* 현실의 나의 위치가 게임의 위치에도 영향을 미친다.
* 게임 시작 전에 자유롭게 돌아 다니더라도, 현실과 동기화된 게임 위치에서 시작한다
![디바이스](/assets/img/posting/2025/VRScale/Tracked Mode - Floor.png)

## 그럼 Floor 로 맞추면 만사 해결되는거 아님????

### 현실과 맞춰서 플레이 할수있어서, 좋아보이지만 단점이 있다.

#### 1. 멀티플레이의 경우, 모든 VR 기기가 동일한 경계를 그리지 않는다면, 게임의 플레이 영역이 다를 수 있다.
* 우리 프로젝트 기준으로 설명해 보겠습니다
#### 12x12 크기의 큰 원룸, 현실 플레이 반경이 4 x 4m일 경우
![디바이스](/assets/img/posting/2025/VRScale/4x4m.png)
#### 현실 플레이 반경이 8 x 8m 일 경우
![디바이스](/assets/img/posting/2025/VRScale/8x8m.png)
* 회색 사각형이 플레이 가능한 반경입니다.

위와 같이 어떤 기기는 4x4m 반경으로 경계를 그리고,
어떤 기기는 8x8m 반경으로 경계를 그린다면 기기마다 각자 플레이 할 수 있는 영역이 달라진다!

#### 2.현실의 플레이 가능한 크기에 따라 플레이 범위의 제약이 생긴다
* 위 4m x 4m의 반경에서도 알 수 있지만, 플레이 가능한 범위가 매우 좁다 (방 크기를 줄이지 않는한..)
* 실질적인 테스트는 유니티에서만 가능할 수도 있을것 같다..
* 룸의 사이즈를 여러개 만든다거나 / 스케일을 동적으로 만들수 있다면 플레이 가능 하겠으나, 좀더 시간이 걸림

#### 3. 게임에서 벽이 있다 해도 유저의 움직임을 제한 할 수 없다.
* 현실과 게임의 구조를 동일하게 만들지 않으면 현실의 움직임을 제약 하지 않는 이상 유저의 위치를 제어 할 수 있는 방법이 없음