---
layout: post 
title: Review - Continuous Hybrid BCI Control for Robotic Arm Using Noninvasive Electroencephalogram, Computer Vision, and Eye Tracking
subtitle: Hybrid BCI Control for Robotic Arm Using EEG, CV, ET
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Brain Computer Interface]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2> Continuous Hybrid BCI Control for Robotic Arm Using Noninvasive Electroencephalogram, Computer Vision, and Eye Tracking 논문 리뷰</h2></div>
<!--break-->

----


 <br/>

## Introduction
현재 다양한 BCI가 발전중이지만 "robotic arm"의 경우 자유도(DOF)가 높기 때문에 BCI를 사용하여 3D 공간에서 여러 물체를 연속적이고 효과적인 방법으로 파악하는 것은 여전히 어려운 문제입니다.

따라서 본 논문은 MI-based EEG, computer vision, gaze detection 기술을 융합한 robotic arm을 제안합니다.
> MI-based EEG의 장점은 SSVEP, P300과 다르게 외부의 자극에 의존하지 않으며 행동과 관련한 직접적인 신호를 전달할 수 있다는 점입니다.

## 2. Methods and Experiments
### A. 2.1. System Architecture
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180722084-51b848eb-ce33-4363-82f7-b28282d3471e.png"
    width="100%"/>
</div>
먼저 task의 큰 흐름을 짚고 각 기술들의 detail을 확인하겠습니다.

참가자는 왼손, 오른손, 양손, 휴식(상상X)을 상상하여 화면 속 virtual ball(2D plane)을 각각 좌,우,상,하로 제어합니다. virtual ball은 robotic arm의 움직임과 같기 때문에 robotic arm도 해당방향으로 움직입니다.

robotic arm은 움직이면서 카메라를 통해 target, obstacle의 위치를 실시간으로 모니터에 feedback합니다.

참가자가 MI를 통해 robotic arm을 움직여 카메라로 target을 인지할 경우(모니터에 target이 뜬 경우) gaze detection 기술로 원하는 target을 선택합니다.

robotic arm은 선택한 target을 자동으로 그립하여 task를 마무리됩니다.

> robotic arm이 움직이는 과정에선 ROS(robot operating system)가 obstacle를 인식하여 알아서 피합니다.

#### BCI System
데이터는 Fs = 500Hz로, 왼쪽 귀밑 reference 전극 포함 20개의 전극이 수집되었습니다.

이 후 50Hz notch filter, common average reference (CAR) filter가 적용되었습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180727843-fc047d6a-0cd4-4db5-8554-91928fc7590c.png"
    width="50%"/>
</div>
> $V_{i}^{CAR}$는 시간 t에서 기준과 i번째 전극 사이의 전위이고, n은 이 연구에 사용된 전극의 수입니다.


각 사용자에 대한 mu frequency band의 진폭을 평가하기 위해 다음과 같이 autoregressive(AR) 모델을 구축했습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180729308-a955ca8f-9a55-4116-899b-1fadd30e8fac.png"
    width="35%"/>
</div>
> $a_{i}$ : weight coefficient

본 논문에선 각 참가자의 sensorimotor rhythm amplitude를 이용해 ball과 robotic arm를 제어합니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180729897-decc157e-4966-4847-b6e6-c92b64571f84.png"
    width="50%"/>
</div>
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180729946-7299874b-7103-4ecd-a1f8-9995289ea9cd.png"
    width="52%"/>
</div>

$K_{V}$ 가 negative or positive일 경우 the virtual ball은  up or down으로 움직입니다.<br/>
$K_{H}$ 가 negative or positive일 경우 the virtual ball은  left or righ로 움직입니다.

$K_{V}$와 $K_{H}$의 weight($w$), offset($d$)은 previous trials에 따라 least-mean-square (LMS) algorithm으로 조정되어집니다.

#### Object Identification and Target Selection
본 논문의 computer vision은 online task의 target selection을 위해 object identification, gaze tracking 기술이 사용됩니다.

target은 4가지의 다른 색상이 존재하며 background subtraction algorithm을 기반에 의해 탐지되어집니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180771570-497a6a91-f533-47b0-9365-2d6d2d67bba9.png"
    width="80%"/>
</div>
먼저 물체의 밝기를 완화하기 위해 RGB를 HSV로 변환합니다. <br/>
이 후 target 이미지의 색상과 임계값의 차이에 따라 식별되어집니다. <br/>
마지막으로 background subtraction에 의해 block이 식별되고 테두리가 생겨 목표물로 표시됩니다.

이 때 사용자는 gaze-control로 마우스를 목표물로 움진인 후 2초간 유지할 경우 선택되어집니다.

#### Robotic Arm Control and Obstacle Avoidance
본 논문은 단순히 EEG data로만 실험했을 경우 정확도도 낮고 정신적인 부담감이 크기에 obstacle을 탐지하고 robotic arm에 정보(point cloud)를 전달할 수 있는 카메라를 workspace에 설치했습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180775426-ba71463b-fd6b-41b5-a174-9c1306efa4cb.png"
    width="100%"/>
</div>
workspace 외부에의 point clouds(정보)는 passthrough filter로 삭제되었습니다. <br/>
voxel grid filter로 계산량을 줄이기 위해 downsampling됐습니다. <br/>
outlier를 제거하기 위해 RadiusOutlierRemoval filter가 사용됐습니다. <br/>
마지막으로 위 데이터는 Octomap로 변환되어집니다. <br/>

결론적으로, robotic arm은 motion planning 단계에서 장애물을 식별하고 피합니다.

### B. Experimental Paradigm
#### MI without Feedback
mian 실험 전, 모든 피실험자실험자마다의 서로 다른 MI의 차이를 maximizing는 최적의 frequencies, spatial channels, action of MI data를 얻기 위한 실험이 진행됩니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180776495-31cd6581-5a79-41a1-b779-4227f9a32f14.png"
    width="60%"/>
</div>
> 상 : 양 손(팔)을 움직이는 상상<br/>
하: 휴식 (아무 상상 x) <br/>
좌: 왼 손(팔)을 움직이는 상상<br/>
우: 오른 손(팔)을 움직이는 상상

해당 실험은 2 session을 포함하고 각 session은 9 run으로 구성됩니다. <br/>
1~3 run은 사진속의 (A)를 상상,<br/>
4~6 run은 (B)를 상상,<br/>
7~9 run은 자신의 임의대로 상상하여 상/하/좌/우에 해당하는 데이터를 수집합니다.

#### Virtual Ball Movement Control Training
MI를 기반으로 robotic arm을 지속적으로 제어하는 능력을 향상시키기 위해 피실험자들에게 MI에 의한 virtual ball control 훈련을 실시했습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180784162-ffef222d-8c75-4560-99c2-a672210690dc.png"
    width="80%"/>
</div>
> stage 1 : Vertical MI task<br/>
> stage 2 : Horizental MI task<br/>
> stage 3 : 2D-MI task

Virtual Ball Movement Control Training은 Virtual Ball을 색칠된 블록으로 이동시키는 task이며 stage 1~3 순서로 진행되었습니다.

#### Online Robotic Arm Control
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180786494-18dbd9b7-f813-4c42-b84a-8eb48054daea.png"
    width="80%"/>
</div>
모니터는 virtual ball movement area와 the USB camera display area로 나눠져있으며 실험 환경은 위 figure와 같습니다. 

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180786684-5b7202fc-322a-4c43-8e68-549bfe3f625b.png"
    width="80%"/>
</div>
실험이 시작되면 피실험자는 3초간 휴식 및 준비합니다. 이후 Virtual ball이 나타나면 피실험자는 MI를 통해 제어합니다.<br/>
Virtual ball이 움직일때마다 camera display도 움직이며 camera display에 target이 나타나면 피실험자는 eye gaze로 마우스를 제어하여 target쪽으로 이동시킵니다.<br/>
마우스를 해당 target에 2초간 유지할 경우 선택되어 robotic arm이 자동적으로 그랩합니다.

세부적인 detail은 아래와 같습니다.
-  task A : one target without an obstacle
-  task B : one target with an obstacle
-  task C : four targets with an obstacle, can select only one asked target

## Results
### A. Performance of MI without Feedback
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180789265-8ead8367-4bcf-40cc-8bc5-ebec47a5814e.png"
    width="100%"/>
</div>
> 피실험자마다 최대 차이를 보여주는 특성을 얻기 위해 진행된 실험

### B. Performance of Virtual Ball Control Training
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180789427-8985ed3b-632a-4f91-9434-fabe65b59b63.png"
    width="100%"/>
</div>
> A : Vertical MI task result<br/>
> B : Horizental MI task result<br/>
> C : 2D-MI task result<br/>
> D : average completion time

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180789954-a06df71e-1f0b-469a-9479-a946fe970664.png"
    width="100%"/>
</div>
> Virtual Ball Control Training을 진행한 후 Online 실험에서 MI의 특성이 더 극대화된 차이를 발생시킴

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180790252-a7728c57-3887-40ee-a223-a64769030260.png"
    width="80%"/>
</div>
> 특히 ERD/ERS의 특징을 확인할 수 있음

### Performance of Online Experiments
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180790569-5eaf3888-b9fe-4d7f-9729-465a4a0463e2.png"
    width="100%"/>
</div>
> Online Experiments의 성공률과 소요시간

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/180791166-f47b7926-4e4e-49aa-936c-bfbc0b164187.png"
    width="70%"/>
</div>
> 각 trial의 time–frequency spectral의 변화 (mu bands에서 큰 ERD/ERS를 볼 수 있음)

## Discussion & Conclusions
본 실험은 사전에 피실험자마다의 최적의 parameter를 구하고 Virtual Ball Control Training을 통해 MI control 능력을 향상시켰습니다.

MI control로 대략적인 위치를 제어한 후 computer vision and eye-tracker기술로 정확하게 타겟팅함으로써 뇌의 부담을 덜고 정확도를 향상시켰습니다.

또한 ROS기술을 응용하여 장애물을 자동으로 피하고 target을 그립할수 있도록 구현되었습니다.

결과적으로 ERD/ERS의 특성을 명백하게 보여줬으며, 사용자가 robotic arm의 이동을 조절할 수 있었기에 MI task는 차후 BCI제어에 필수적인 요소가 될것이라 주장합니다.

---

## 고찰
not yet


###### Reference
* DOI: 10.3390/math10040618