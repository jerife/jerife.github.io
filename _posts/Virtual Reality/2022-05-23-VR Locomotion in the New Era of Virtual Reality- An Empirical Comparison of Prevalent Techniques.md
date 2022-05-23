---
layout: post 
title: Review - VR Locomotion in the New Era of Virtual Reality An Empirical Comparison of Prevalent Techniques  
subtitle: Virtual Reality Locomotion
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Virtual Reality]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2>VR Locomotion in the New Era of Virtual Reality- An Empirical Comparison of Prevalent Techniques 논문 리뷰</h2></div>
<!--break-->

----


 <br/>

## ABSTRACT
해당 Article은 3가지의 VR Locomotion techniques을 mixed methods approach에 기반한 사용자의 경험적인 요소(UX)를 평가합니다.

위 실험을 통해 아래와 같은 결과를 도출합니다.
> (i) walking-in-place: 높은 몰입성을 보이지만 신체적으로 쉽게 지침<br/>
> (ii) controller/joystick: 친숙한 기법이기 때문에 쉽게 조작 가능<br/>
> (iii) teleportation: 빠르게 이동할 수 있지만 시각적으로 사용자의 몰입성이 떨어짐


## INTRODUCTION
논문의 목적은 널리 사용되는 VR Locomotion를 비교하고 경험적인 평가 연구를 제시합니다. 궁극적으론 새로운 VR Locomotion의 기술을 개발하고 연구에 기여하기 위함입니다.


## BACKGROUND
위 논문은 이전 VR Locomotion에 대해 평가, 비교한 논문들을 기반으로 VR Locomotion을 실험하고 평가했습니다.

해당 Chapter에는 각 VR Locomotion와 이를 다룬 논문들이 기재되어 있으니 참고하시길 바랍니다.


## Prevalent VR Locomotion Techniques
시작에 앞서 가장 널리 사용되고 있는 4가지 VR Locomotion를 언급합니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169730451-4f004639-e259-4045-819a-62374658e9a7.png" width="100%"/>
</div>

> (i) Motion-based: 신체를 이용해 continuous하게 interact함 <br/>
> (ii) Room-scale-based: Motion-based와 같게 interact하지만 실제 환경의 규모와 사이즈에 제한을 받음<br/>
> (iii) Controller-based: Controller를 기반으로 continuous하게 interact함 <br/>
> (iv) Teleportation-based: Controller로 순간이동을 하면서 discrete하게 interact함 <br/>

원래 가장 널리 사용되는 4가지 VR Locomotion지만, <br/>
(ii) Room-scale-based의 경우 실제 환경의 물리적 한계요소가 있기 때문에 VR Space기반인 나머지 3가지를 이용해 비교, 평가 합니다.

## METHODOLOGY
본 연구는 널리 보급된 세 가지 VR Locomotion을 평가하고 향후 관련 설계에 사용할 수 있는 피드백을 받아 어떤 경험적 요인, 경험적 품질 및 시스템 특징이 VR 이동과 가장 관련이 있는지 조사합니다.

### A. Materials
#### VR Locomotion Techniques
- HTC Vive headset
- SteamVR SDK for Unreal Engine 4 by Epic Games
- Body tracking할 수 있는 적외선 sensors
- Body tracking할 수 있는 HTC Vive tracker
- walking-in-place and controller/joystick 의 경우 speeds를 (1~4-3m/s)로 제한

###### (i) Walking-in-place
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169732803-56492366-e18d-493a-b5ce-c5b4f4c55bf6.png" width="50%"/>
</div>
 오른쪽 발에 HTC Vive tracker 부착, HMD를 기준으로 화면이 시각적으로 보여짐 방향을 바꾸기 위해선 몸을 돌아 움직어여함

###### (ii) Controller/joystick
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169733447-a4badc37-83b6-4ae7-9037-49b8fe2766b4.png" width="50%"/>
</div>
Walking-in-place와 방식은 같지만 컨트롤러를 이용해 움직이는 차이점이 있습니다.

###### (iii) Teleportation
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169733619-7fb807be-6cc4-44a3-9c71-04ea8d99c0f7.png" width="50%"/>
</div>
Teleportation의 경우 버튼을 누르고 화면에 보이는 mark된 위치로 이동하는 mechanism입니다.

#### Questionnaires and Interview
데이터 수집 시작 단계에 Subjects의 age, gender, frequency of VR use와 같은 기본적인 정보를 수집했습니다.

연구의 데이터는 Game Experience Questionnaire, System Usability Scale questionnaire, Semistructured interview로 수집되고 평가 되었습니다.

###### (i) Game Experience Questionnaire (GEQ)
사용자 경험 설문이며 넓은 범위의 요소에 좋은 신뢰성을 보이기 때문에 다양한 domain에서 사용되었습니다. 설문은 총 16문항으로 0~5점까지 부여될 수 있습니다.

VR 도메인에선 navigation, locomotion, haptic interaction, VR learning, cyberpsychology, VR gaming 등에 사용되었습니다.

기술 평가의 요소로는 Competence, Sensory, Imaginative Immersion, Flow, Tension, Challenge, Negative Affect, Positive Affect, Tiredness등이 가 있습니다.

###### (ii) System Usability Scale (SUS)
SUS는 참여자와 연구자의 생산품과 서비스의 주관적인 사용성을 측정할 수 있는 기법입니다. 또한 소수의 subject에 견고한 결과를 낼 수 있다는 장점이 있습니다.

VR 도메인에선 VR rehabilitation and health services, VR learning, VR training 등에 사용됐습니다.

SUS는 10개의 문항으로 0~100점까지 부여될 수 있습니다. 점수는 형용적으로(good, ok, bad, etc)등으로 표현될 수 있으며 A~F로 매핑할 수 있습니다.

###### (iii) Semistructured interviews
semistructured interview는 subject들에게 각 VR Locomotion에 장단점이나 그 이유등을 물어보고 comment를 받는 과정입니다.

### B. Environment
연구는 'Simple Town’라는 가상환경에서 진행되었습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169740146-1f3d614e-70cd-4112-9382-f0d01b36ee17.png
" width="50%"/>
</div>
Subject는 특정장소를 다녀야하는 task를 수행해야합니다. (eg. 먼저 자동차 수리점을 들렸다가 영화관을 가세요...) <br/>
사진속에선 빨,파,초 3가지 task이며 각 task는 다른 VR Locomotion 기술을 사용해 실험합니다.<br/>
각 task는 7~15분 동안만 진행할 수 있습니다.

### C. Procedure
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169748562-84513fba-44ed-44a4-9fdb-320f60b316f7.png" width="100%"/>
</div>
먼저 Subject에게 연구에 관한 정보를 숙지시킨 뒤, 그들의 기본적인 정보 및 VR 경험에 관한 설문을 진행합니다.<br/>
그 후 Subject에게 VR을 경험할 수 있는 시간(연습시간) 5~7분정도를 제공한 후 task가 실행됩니다. 실험중에 Subject는 구두로 feedback및 인터뷰를 위한 메모를 할 수 있습니다.<br/>
한 task가 종료되고 SUS and GEQ 설문을 시작하고 5분정도 휴식 후, 다음 task를 진행됩니다. 3 task가 모두 끝난 후 Subject는 Semistructured interview를 진행합니다.

### D. Statistical Analysis
모든 데이터는 SPSS를 이용해 분석되었습니다. 유의 수준은 $p$ < 0.05로 설정되었습니다.
참가자의 GEQ, SUS 값을 분석하기위해 기술분석이 사용되었습니다.

- 프리드먼 검정으로 두 값의 차이를 탐지합니다. 
- post-hoc, pair-wise 분석을 위해 Wilcoxon signed rank test이 적용됩니다.
- 인터뷰 데이터는 핵심 개념, 주제 및 아이디어가 식별되어져 open, axial 코딩으로 NVivo로 분석됩니다.

### E. Results
#### Game Experience Questionnaire (GEQ)
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169748626-932294c0-56e4-4b5f-b862-23cbf4ef7c40.png" width="100%"/>
</div>

위 표는 Wilcoxon signed rank test를 사용한 GEQ 점수의 post-hoc 분석 결과를 제시합니다.


<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169751396-adb20dfc-fe92-448b-92e3-2a451c4ac662.png" width="100%"/>
</div>

위 도표는 GEQ 설문지의 평균 값을 의미합니다.

#### System Usability Scale (SUS)
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169752331-588c5f47-f655-41b6-865a-19863180ab53.png" width="100%"/>
</div>

SUS에서 얻어진 결과를 형용된 평가와 점수를 기반으로 묘사해놓은 표입니다.

post-hoc 분석은 WIP and Controller (𝑍 = −3.833, 𝑝 < 0.001), WIP and Teleportation (𝑍 = −3.393, 𝑝 = 0.001)
두 케이스의  pair-wise 분석에서 유의미한 것을 확인할 수 있으며, WIP는 다른 두 기술보다 낮은 SUS 값을 띄었습니다.

#### Semistructured interviews
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/169753791-35497b25-142a-47b6-8a62-18297994089f.png" width="100%"/>
</div>

WIP의 경우 실제로 움직이는 것에 높은 몰입도를 제공했으며 운동, 재미, 오락등의 특징성이 있음을 발견하기도 했습니다.<br/>
반면 움직이는 것이 성가시다는 의견과 시각적인 제한으로 인한 두려움, 멀미 유발등의 의견도 있었습니다.

Controller/Joystick의 경우 친숙하고 직관적이고 편하다는 의견이 있었으며 조작법을 빠르게 숙지할 뿐아니라 만족스러운 몰입도를 제공한다는 의견도 있었습니다.

Teleportation의 경우 몰입도가 가장 낮다고 보고되었습니다. 이유로는 시각적으로 점프와 불연속적으로 움직이기 때문입니다. 또한 텔레포트 이후의 화면은 시각적으로도 부담이 된다고 합니다. <br/>
하지만 빠르고 효율적으로 이동할 수 있다는 장점과 숙지하는데 어렵지 않다는 의견이 보고됐습니다.

## DISCUSSION 
### A. Walking-in-Place
WIP(Walking-in-Place)는 GEQ의 Tension, Challenge, Negative Affect, Tiredness에서 높은 값을 얻었으며 SUS의 값이 낮았습니다.

Subjects는 두가지의 정반대 의견을 냈습니다.
1. 반복적으로 움직이는 것은 지루함을 초래하며 HMD으로 인해 앞이 보이지 않는 상황에서 부딪칠지도 모른다는 두려움이 있음
2. VR 컨텐츠에 더욱 흥미로운 요소가 추가된 것 같음

반복적으로 움직이는 것은 지루함을 초래하는 것은 이전 연구들과 공통적으로 발견된 결과이지만, 이번 실험을 통해 'HMD으로 인해 앞이 보이지 않는 상황에서 부딪칠지도 모른다는 두려움이 있음'라는 새로운 발견이 있었습니다.

궁극적으로 WIP는 VR과 강한 연결성을 띄우고 있으며, User가 사전에 체력소모가 있다는 것을 알고 시작한다면 흥미를 느낄 수 있을 것이라 했습니다.<br/>
추가적으로 운동이나 게임같은 행동이 필요한 콘텐츠에 접목시킬경우 큰 장점을 발휘할 수 있을 것이라는 결론을 제시합니다.

### B. Controller/Joystick
Controller/Joystick은 GEQ의 Competence, Immersion,
Flow, Positive Affect에서 중간~높은 값을 얻었으며 최상의 SUS의 값을 얻었습니다.

이유를 분석해본 결과 조작이 쉬우며 금방 숙련될 수 있기 때문입니다. 또한 높은 편리성을 보여주지만 GEQ의 Tension, Negative Affect, Tiredness에서 낮은 값을 얻었습니다.


### C. Teleportation
Teleportation의 경우 조작이 쉽고 효과적이라는 결과가 도출됐습니다.

하지만 인터뷰결과를 통해 멀미가 심하다는 부정적인 인식이 있었으며 그 이유로는 점프로 날아가는 모션이나 갑자기 화면이 바뀌어 시각적으로 부담스럽기 때문이었습니다.

GEQ Immersion and Flow에서 낮은 값을 부여받았으며 중간정도의 Challenge, 중간~낮음정도의 Negative Affect, Tiredness는 상호작용과 관련되 문제일 수도 있음을 제시했습니다.


## CONCLUSION
1. 본 연구는 경험적으로 사용자 중심의 관점에서 각 VR Locomotion의 속성을 문서화하고, 
2. 이것이 미래 VR Locomotion 연구,개발에 영향이 갈수 있도록 의미를 제시하고, 
3. 사용자가 상호작용하면서 중요하게 여겨지는 4가지 dimensions을 비교하는 것에 집중 되었습니다.
> 4 dimensions: Immersion, Ease-of-use and mastering, Competence and sense of effectiveness, Psychophysical discomfort

논문은 향후엔 각 VR Locomotion의 개발과 발전에 대해 다룰 것이며, 4가지 dimensions을 측정할 수 있는 더 나아가 연구 툴 개발을 위해 연구할 것이라며 마무리됩니다.

---

## 고찰
3가지 VR Locomotion 들은 각각의 Trade-off한 점이 있으며 그럼에도 가장 고전적인 방법중 하나인 Controller/Joystick가 가장 좋은 SUS 결과를 내는 것은 아직 VR Locomotion에 혁명적인 기술이 없기 때문이며 더욱이 BCI와 VR을 접목시키는 것이 필요하다고 생각이 들었다.

###### Reference
*  https://www.hindawi.com/journals/ahci/2019/7420781/