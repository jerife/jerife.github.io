---
layout: post 
title: Review - The Effect of Multisensory Pseudo-Haptic Feedback on Perception of Virtual Weight
subtitle: Multisensory Pseudo-Haptic
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Virtual Reality]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2> Multisensory Pseudo-Haptic Feedback on Perception of Virtual Weight 논문 리뷰</h2></div>
<!--break-->

----


 <br/>

## ABSTRACT
virtual objects에 데한 haptic feedback은 몰입적인 VR경험에 중요한 역학을 하지만 대부분의 haptic feedback은 stimuli 인식이 제한됩니다.(narrow modulation range of simulated perception)

따라서 본 논문은 control-to-display (C/D) ratio와 electrical muscle stimulation (EMS)를 기반으로 하는 multisensory pseudo-haptic feedback를 제안합니다.

결론적으로 multisensory를 사용할 경우 absolute thresholds(인지할 수 있는 기준)관점에서 기존의 기법보다 2배이상의 성공적인 결과를 보여주며,<br/>
difference thresholds(변화를 감지할 수 있는 기준)관점에서 C/D ratio가 감소할수록 인지능력이 올라간다는 것을 확인할 수 있었습니다.

본 연구는 VR에서 시뮬레이션된 체중 인식에 대한  multisensory pseudo-haptic feedback의 심리적 영향을 이해하는 데 기여합니다.

## INTRODUCTION
VR은 새로운 경험과 기회를 제공할 수 있는 잠재력을 갖고있습니다. 하지만 각종 domain에 적용되기 위한 가장 중요한 문제는,<br/>
VR에서 user가 실제 세계에서처럼 감각을 높이고, 더 많은 정보를 얻고 적절하게 반응하도록 하기 위해 더 몰입적이고 현실적인 haptic 경험을 제공해야 한다는 것입니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172044949-c6dcc968-8a54-4cde-8ae2-5704b2bf6b90.png" width="100%"/>
</div>

그 중 무게를 인지하는 것은 중요한 haptic의 가치입니다. 이를 인지시키는 방법에는 체감각, 시각, 청각적인 감각등을 이용하는 방법들이 있습니다. 기존엔 단일 감각만을 사용했지만 본 논문에선 두 감각을 사용한 pseudo-haptic feedback을 제안합니다.

시각적으론 (C/D) ratio를 이용하며 체감각적으론 EMS를 이용해 stimuli를 전달합니다.

> **[(C/D) ratio]** : 실제 손이 움직이는 속도와 가상의 손이 움직이는 속도<br/>
(eg. 마우스를 천천히 움직이면 커서가 조금만 이동함 / 마우스를 빠르게 움직이면 커서가 많이 이동함 )

> **[EMS]** : 전기적 자극으로 근육에 신호를 전달하는ㄴ 기술

## DESIGN OF MULTISENSORY PSEUDO-HAPTIC FEEDBACK
### A. VISUAL PSEUDO-HAPTIC FEEDBACK DESIGN
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172045278-d128c433-ade5-423c-a3bb-0ef872d420e6.png" width="80%"/>
</div>
많은 Visual pseudo-haptic feedback 연구들은 실제 움직임과 가상의 움직임 사이의 mismatch를 이용해 무게를 인지하는 것을 착각시킵니다.
> eg. 가상에서 무거운 물체를 들면 가상의 손은 실제의 손보다 천천히 움직임

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172045121-171bfb0c-2b03-4283-af85-d149d98e9fda.png" width="50%"/>
</div>
본 논문은 linear interpolar (1)식을 이용해 VR(3D차원)에서 적용 되는 (2)식을 사용했습니다.<br/>
실험에선 C/D ratio를 0~1 값으로 조정하면서 실험에 임했습니다.
> C/D ratio가 높으면 빠르게 움직임<br/>
C/D ratio가 낮으면 느리게 움직임(더 많은 저항을 받음)

### B. SOMATOSENSORY PSEUDO-HAPTIC FEEDBACK DESIGN
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172045338-aec6532b-7f0e-45fe-98b8-6f7f027b03a0.png" width="80%"/>
</div>
EMS를 사용한 많은 연구들이 virtual object의 무게를 인지시키고자 삼두근과 이두근을 사용했습니다.

본 논문도 마찬가지로 user의 팔이 아래로 내려가고 가상 물체를 들어올리기 어렵게하기 위해 삼두근에 근육 수축과 이두근에 이완할 수 있는 자극을 줬습니다.

EMS의 자극 정도는 사전 실험에 의해 user마다 적당한 정도가 부여됩니다. 또한 이 자극은 ON/OFF로 작동되며 Unity의 REST API를 사용해  virtual object를 들을 경우 hardware(EMS)가 동작되도록 설계됬습니다.
> EMS의 평균 자극 정도: 17 mA (range: 10 30 mA)

### C. EXPLORATORY EXPERIMENT
이후 C/D ratio, EMS의 영향력을 파악하고 적절한 C/D ratio의 범위를 지정하는 목적으로 탐색적 실험이 진행됐습니다.

실험은 C/D ratio를 변화하면서 (0.005, 0.05, ..., 0.6, 1.0) 랜덤한 순서로 EMS를 on/off합니다. 이후 Witmer-Singer presence questionnaire의 sensory, realism, distraction, control factors에 대한 경험을 설문합니다.

1. How compelling was your sense of objects moving through space? (Sensory Factor)
2. How much did the simulated weight on the virtual object seem consistent with your real-world experiences? (Realism Factor)
3. How distracting was the control mechanism? (Distraction Factor)
4. How much delay did you experience between your actions and the expected outcomes? (Control Factor)

실험이 시작되면 Participants는 오른손으로 가상의 큐브를 어깨높이로 들게하고 3초간 경험하게한 후, 무게감을 느꼈다면 가상의 "yes"존(zone)에 못 느꼈다면 가상의 "no"존에 큐브를 내려 놓습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172045796-942e72f3-a027-4433-a4f1-a0b836c1d8be.png" width="100%"/>
</div>
absolute threshold(무게 인지 확률 0.5를 기준으로 무게를 인지하고 못하고를 판단)에 의해<br/>
EMS ON의 경우 C/D ratio 0.5, EMS OFF의 경우 C/D ratio 0.2이상 에서 무게가 인지된다는 결과를 도출했습니다.

또한 C/D ratio가 0.005이하일 경우 안좋은 부정적인 영향을 미쳐 다음 실험에선 0.005이상으로 실험할 예정입니다.

## EXPERIMENT 1: ABSOLUTE THRESHOLD OF WEIGHT
위 실험은 EMS를 결합한 multisensory feedback이 modulation range를 더 넓게 해줄 것이라는 가정하에, 무게 인지의 absolute threshold를 측정합니다.

### B. PROCEDURE
실험 1은 EXPLORATORY EXPERIMENT와 같으며 다른점은 아래와 같습니다.
- 0.005이하의 경우 의미가 없는 결과가 나오기 떄문에, C/D ratio values를 (0.05, 0.1, 0.15, 0.2, 0.4, 0.7, 1)로 변경합니다.
- 무게 인지의 absolute threshold를 정확하게 평가를 위해 각 조건마다 20번씩 실험합니다.(총 실험: 20 * 2 * 7 = 280)
- default 무게를 상기시키기위해 task를 시작하거나 매 40번쨰 실험마다 아무런 feedback(시각적, 체감각적 feedback)이 없는 실험을 합니다.
- 이후 Witmer-Singer presence questionnaire의 comfortableness, disembodiment에 대한 설문을 하고 간단한 인터뷰를 진행합니다.

### C. RESULTS
#### 1) WEIGHT PERCEPTION TASK
실험 결과, exploratory experiment에서 얻은 결과와 유사하게 EMS가 높은 무게 인지 확률을 제공한다는 결과를 얻을 수 있었습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172047755-5e6206dd-d58a-44b0-91e6-51644736bb58.png" width="60%"/>
</div>
또한 무게 인지의 absolute threshold 결과에 의해 multisensory pseudo-haptic feedback이 무게인지의 modulation range를 2.5time 가량 더 넓힐 수 있다는 것을 발견했습니다.
> multisensory(visual+EMS) : C/D ratio = 0.46<br/>
only visual sensory: C/D ratio = 0.18

#### 2) SURVEY
Wilcoxon signed-rank test를 사용해 single, multisensory에 대한 comfortableness score와 disembodiment score를 비교한 결과 대부분이 multisensory이 좀더 편하다고 했지만 불편하다는 의견이 있기도 했습니다.

#### 3) INTERVIEW
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172047784-e063d806-5da3-4bac-ae7c-a9495f55a5aa.png" width="100%"/>
</div>
INTERVIEW 결과 single의 EMS가 무게인지에 도움이 되기도 했다고 합니다. 하지만 SURVEY와 마찬가지로 불편하거나 친숙하지 않다는 결과도 있습니다.

## EXPERIMENT 2: DIFFERENCE THRESHOLD OF WEIGHT
실험 1의 absolute threshold는 사용자가 pseudo-haptic feedback에 의해 시뮬레이션된 무게의 작은 차이에 얼마나 민감한지에 대한 정보를 제공하지 않습니다.

따라서 실험2는 두 큐브의 무게를 비교할 수 있는 standard C/D ratio에 따른 difference threshold(변화를 느낄 수 있는 임계값)을 구하는 실험을 진행합니다.

### A. PROCEDURE

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172048089-7931bac1-52c6-46ed-b6b5-c7de8932bf6a.png" width="80%"/>
</div>

participants는 두 큐브를 들어 무거운 것을 위 사진속의 "Heavy"존에 내려 놓거나 둘다 무게가 같을 경우 원래 자리에 내려놓습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172048104-df1f438c-9d61-49a4-93ec-7e29fd04f91f.png" width="60%"/>
</div>
> maximum likelihood procedure (MLP)알고리즘에 의해 랜덤하게 C/D ratio가 배정되는 과정

두 큐브중 하나의 C/D ratio는 standard C/D ratio로 지정됩니다. 또 다른 큐브는 maximum likelihood procedure (MLP)알고리즘에 의해 랜덤하게 standard C/D ratio보다 작은 C/D ratio가 배정됩니다.

- standard C/D ratio의 값은 (0.05, 0.1, 0.15, 0.2, 0.4,
0.7, 1)로 랜덤한 순서로 EMS은 On/Off됩니다.
- 실험은 각 조건마다 18회 진행됩니다. (총 실험: 18 * 2 * 7 = 252)
- 총 18번동안 MLP 알고리즘에 의해 랜덤하게 C/D ratio가 배정될때 총 18번중 네번은 가장 작은 0.001로 배정됩니다.(계속 인지할 수 있게하기 위함)

### C. RESULTS
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172048979-cdbabb0b-a447-4450-b623-4a42143dabb3.png" width="60%"/>
</div>
> eg. standard C/D ratio가 0.2일떄 적어도 0.07의 C/D ratio 차이가 있어야 무게가 다르다는 것을 감지할 수 있음

전반적으로 standard C/D ratio가 증가함에 따라 user는 무게에 대한 변화를 느끼기 위해서 더 큰 C/D ratio를 요구했습니다.

결과를 보면 EMS를 추가한다고 해서 큰 영향이 있지는 않았습니다. <br/>
하지만 실험 2는 standard C/D ratio를 조절할 것이고 EMS의 자극 정도롤 조절 한것이 아니기 때문이라고 설명될 수 있습니다. 따라서 EMS를 조절하는 추가적인 실험이 필요하다고 합니다.

종합하면, 실험 2의 결과는 EMS on/off에 따른 standard C/D ratio 값에서 무게의 difference threshold가 어떻게 변화하는지 보여줍니다.

## GENERAL DISCUSSION
실험 1에서 multisensory pseudo-haptic feedback이 무게 인지의 modulation range를 더 넓게 해준다는 가설과, <br/>
실험 2을 통해 user의 무게 변화 인지는 feedback에 따라 달라질 수 있다는 가설을 세웠으며 각각 아래와 같은 결과를 도출했습니다.

- absolute threshold 실험에 의해 multisensory가 더 효과적인 가상의 무게를 인지시켜준다는 것을 확인했습니다.
- difference threshold 실험에서는 standard C/D ratio가 증가할수록 difference threshold가 증가한다는 것을 확인했습니다.

또한 실험 1,2에서 얻어진 absolute, difference threshold의 결과를 아래와 같이 활용할 수도 있습니다.
- signle pseudo-haptic feedback(only visual)만을 사용해 A,B라는 물체에 0,05, 0,1 C/D ratio를 적용할 수 있습니다.
- 또한 EMS를 추가한다면 C,D라는 물체에 0.2, 0.4 C/D ratio를 적용할 수 있습니다.

### A. LIMITATION AND FUTURE WORK
본 실험의 한계로는 다음와 같은 요소를 언급하며 마무리 됩니다.

- 본 실험에선 Participants 개인마다 EMS를 느끼는 level이 다를뿐 아니라, survey와 interview를 통해 높은 level은 불편함을 초래하기 떄문에 EMS를 binary하게(ON/OFF) 적용했을 떄 추가적인 영향을 미치는지에 집중 되었습니다. 만약 다양한 level의 EMS가 적용 됐다면 absolute, difference threshold에 다른 영향을 미쳤을 것이라 합니다.

- C/D ratios를 linear하게 적용했다는 점입니다. 만약 non-linear하게 적용했다면 difference threshold실험에서 더 다양하게 적용할 수 있었을 것이라 하며, 이후에는 C/D ratio를 non-linear하게 적용하고 1.0이상을 적용해보는 실험을 제안합니다.

- controller(129 g) 무게도 영향을 미칠수도 있다고합니다. 하지만 본 실험에선 이것이 실험에 미치는 영향을 배제했다고 합니다. 그리고 차후 VR application의 hand-tracking 기술 발전은 맨손으로 상호작용할 수 있게 해주기에 중요하다고 언급하며 마무리됩니다.

---

## 고찰
VR과 BCI를 접목함에 있어 완전히 BCI만 활용해 제어하는 것이 아닌 haptic, motion tracking등과 같이 활용할 수 있을 것 같다는 생각이 들었다.

아직 BCI 단계가 완전몰입가상현실을 구현하기엔 한계가 있기 떄문에 사용자에게 몰입적인 경험을 제공하기 위해선, locomotion같은 이동기술이나 간단한 task(eg. 환결설정 화면 on/off)등은 BCI를 적용하고 손동작(eg. 총을 잡는 자세, 물건을 드는 자세)등은 haptic, motion tracking의 기술을 접목시키며, user가 힘을 느낄 수 있도록 본 논문과 같은 Multisensory Pseudo-Haptic을 적용할 수 있을 것 같다.


###### Reference
* DOI: 10.1109/ACCESS.2022.3140438