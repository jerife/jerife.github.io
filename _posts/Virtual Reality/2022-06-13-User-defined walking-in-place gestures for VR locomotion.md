---
layout: post 
title: Review - User-defined walking-in-place gestures for VR locomotion
subtitle:  Walking-In-Place for VR locomotion
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Virtual Reality]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2> User-defined walking-in-place gestures for VR locomotion 논문 리뷰</h2></div>
<!--break-->

----


<br/>

## ABSTRACT
Locomotion은 가장 기본이되는 VR interaction이며, VR Locomotion기술 중 Walking-in-place (WIP)는 실제로 걷는것과 같은 장점이 있습니다. 

하지만 대부분의 WIP gestures는 user가 아닌 개발자의 관점에서 개발되었으므로, 학습 및 암기에 대한 인지 부하가 높을 뿐만 아니라 VR에서 존재감이 악화되고 감각 충돌이 증가할 수 있습니다.

따라서 본 연구에선 users로부터 WIP gestures를 도출해내고 평가하는 것을 목표로 합니다.

## INTRODUCTION
WIP는 실제 보행보다 선호도가 낮지만, 실제 보행과 동일한 물리적 공간이 아닌 가상 공간을 위한 작은 물리적 공간이면 충분합니다.

또한, 대부분의 경우 hand-free할뿐 아니라 실제 보행과 유사한 움직임으로 전정 및 고유 감각 신호를 제공하여 기존 조이스틱 인터페이스에 비해 더 나은 몰입감과 공간 지각을 제공합니다.

기존의 WIP는 아래와 같은것 들이 존재합니다.
- Stepping-in-place (SIP): 제자리를 걸어서 control
- Wiping-in-place : 바닥을 쓸어서 control
- Tapping-in-place : 발끝을 대고 발뒤꿈지만 들어서 control 
- Swing-in-place : 한발을 들고 몸을 기울여서 control
- Jogging-in-place : 조깅하듯이 control
- 그밖에도 상체나 머리를 기울여서 control하거나 등의 방법이 존재

하지만 이외의 다양한 방향을 포함하여 서 있는 동안의 WIP gestures를 유도한 연구는 없습니다.<br/>
또한 user가 앞으로 걷기에 step같은 WIP gestures를 선호하는지 의심스러우며, 옆, 뒤 걷기에 자연스럽고 사용하기 쉬운 gestures는 아직 알려지지 않았습니다.

따라서 본 연구는 user의 관점에서 앞, 옆, 뒤로 걷는 전체 방향을 포괄하는 완전한 WIP gestures 세트를 생성하는 것을 목표로 합니다. user가 도출해낸 gestures를 수집, 그룹화 및 평가합니다.

## Experiment 1: Elicitation of WIP gestures for VR locomotion
### 2.1. Method
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172992731-815a4ab7-74b5-4dd0-959b-16556195935d.png" width="80%"/>
</div>

실험의 모든 commands는 얼굴이 항상 정면을 향하고 가상 환경에서 해당 방향으로 걷는 것을 나타냅니다.

실험은 2가지 순서로 진행됩니다.
1. user가 직접 WIP gestures를 도출하는 실험
2. 기존에 존재하는 WIP gestures 방식 3가지(Tapping-in-place / Jogging-in-place / Swing-in-place)를 실험하고 0~7점까지 점수 부여

#### Procedure
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/173000926-62e9ebda-0e07-4a4b-875d-f58e397ac12a.png" width="100%"/>
</div>
> Reference Video: https://vimeo.com/527773000

먼저, 참가자들에게 VR 헤드셋을 가슴 앞에 대고 8방향으로 각각 3보씩 2회 걷게 하고, 두 번째 시도에서의 보행 속도는 VR 헤드셋의 위치 추적으로 측정합니다.

이후 참가자들은 VR 헤드셋을 착용하고 무작위 순서로 3초 동안 8가지 명령을 각각 도출합니다. 앞서 측정한 속도에 따라 보행 속도를 설정하여 WIP gestures 유도 시 자연스러운 느낌을 제공했습니다.



WIP gestures가 완료되면 센서 카메라로 gestures를 기록하고 디자인 이유 및 보행 속도를 높이는 방법에 대한 구두 설명도 함께 기록합니다.

마지막으로, 참가자들은 명령을 표시하는 동안 3-4회 걷기 주기 동안 세 가지 기존의 WIP gestures 각각을 수행합니다.<br/>
각 gestures를 두 번 수행한 직후 참가자들은 3가지 주관적 평가 항목인 7point Likert 척도(1=매우 동의하지 않음, 7=매우 동의함)로 평가하도록 요청받았습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/173268477-06c3c502-83ce-48bd-9558-d744176b01d4.png" width="60%"/>
</div>
> Wizard of Oz technique 란? 실제로 구현되는 것이 아닌 상황을 보고 pilot이 직접 작동시키는 방식

VR에서의 움직임은 Wizard of Oz technique로 구현됩니다.


#### Data analysis
20명의 참가자가 8 commands를 수행해 총 160 WIP gestures 데이터를 수집했습니다.

유도된 gestures는 먼저 direction과 movement 두 가지 구성요소로 분해하였고, gestures의 움직임 특성과 영상에 기록된 참여자의 구두설명을 고려하여 그룹화합니다.<br/>
그런 다음 각 명령에 대해 일치율(AR)을 계산하여 다양한 참가자로부터 도출된 gestures 간의 일치 정도를 평가했습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/172996771-5c60317f-9928-4cc0-9676-6cc6c7e7295f.png" width="50%"/>
</div>
> $P$ : c에 대해 유도된 gestures의 총 수<br/>
> $P_{i}$ : P에서 동일한 gestures의 하위 집합 i의 수

- AR < 0.1일 때 일치
- 0.1 ≤ AR < 0.3일 때 중간 일치
- 0.3 ≤ AR < 0.5일 때 높은 일치
- AR ≥ 0.5일 때 매우 높은 일치

기존에 존재했던 gestures 3가지는 ANOVA와 post hoc Tukey tests을 통해 비교됐습니다.

### 2.2 Results
#### Grouping results and gestures characteristics
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/173000201-1bc4eff1-7f81-44cf-992e-18bb04045bfb.png" width="100%"/>
</div>
Table 1의 표와 같이 direction은 8개의 group, movement는 14개의 group이 도출되었습니다.

direction 중엔 Turn body, Step one foot<br/>
movement의 경우 SIP, Rock, Stay가 우세적이었습니다.

디자인 이유에 따르면 참가자의 50%는 실제 걷는 것과 유사한 느낌을 주기 위해 동작 특성을 가장 많이 모방하고, 35%는 특정 방향으로 움직이는 느낌을 더 중시한다고 설명했습니다. 나머지는 gestures를 수행할 때 편안함을 극대화하는 것을 고려했다고 합니다.

#### Comparison between existing gestures
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/173000870-6fbc689d-4ed7-4e9c-b033-fa5b686dd8c4.png" width="80%"/>
</div>
Tapping-in-place gestures가 goodness of fit, ease of use, perceived fatigue 부분에서 outperform 했습니다. 

### 2.3 Discussion

direction는 대부분의 경우(80%) 참가자가 몸을 돌리거나 해당 방향으로 한 발을 내디뎠습니다.

Turn body는 전후방향으로 방향을 나타내기 위해 한 발보다 많이 선택되었고, 옆으로(±45º, ±90º, ±135º) 방향에서는 반대였습니다.<br/>
90º 방향에서 더 많은 참가자가 몸을 90º로 돌리는 것보다 먼저 가까운 발을 들어 다른 쪽 발보다 높게 만들어 방향을 표시하는 것을 선호했습니다.
> 사람의 목은 80º까지 움직이는게 가능하기 때문에 자연스럽게 몸을 돌리는 것은 불편을 초래함

movement의 경우 흔히알려진  SIP gestures가 가장 우세했습니다.<br/>
한 가지 흥미로운 점은 0º 이외의 방향에서 SIP gestures를 사용하는 경향이 있다는 것입니다. 
> 참가자가 지식과 경험을 새로운 지식과 경험으로 다양한 gestures를 시도한다는 것

모든 gestures를 조사하는 대신 평균 2명 이상의 참가자가 제안한 gestures가 선택되었으며, 독특하게 디자인된 인기 없는 gestures는 더 나은 학습성과 기억 가능성을 위해 제외되었습니다.

추가로 existing gestures에서 가장 효과적이었던 Tapping-in-place gestures가 함께 조사되어집니다.

따라서 실험 2에서 아래의 WIP가 조사되어질 예정입니다.
- Turn body + SIP (TSIP)
- Step one foot + SIP (SSIP)
- Step one foot + Rock (Rock)
- Step one foot + Stay (Stay)
- Exist (TIP)

## Experiment 2: Follow-up evaluation of user-elicited and existing gestures sets
### 3.1. Method
#### Experimental settings and procedure
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/173268794-9ba1f1eb-8507-4eb9-91f0-254a9892b0d7.png" width="100%"/>
</div>
> Reference Video: https://vimeo.com/469634346

실험 세팅을 실험 1의 환경과 같게 구축했으며, 이동 속도의 경우 실험 1의 참가자들의 평균 이동속도를 사용했습니다.

먼저 참가자에게 실험 1에서 도출된 5가지 WIP를 video와 설명을 통해 이해시킵니다. 이후 gestures의 평가과정은 실험 1과 같게 진행됩니다.

실험 2에서 추가된 점은 gestures를 수행하기 전 / 후의 위치를 측정해 분석한다는 점입니다.

#### Data analysis
주관적인 평가 항목의 등급과 gestures 및 방향의 positional drift를 수집하여 통계적 분석을 위해 정리했습니다.

outlier를 찾기위해 Grubbs’ outlier test로 검증하고 outlier가 존재할 경우 제외시켰습니다.<br/>
Ryan-Joiner test를 통해 normality(정상상태)를 검증하고 정규분포를 따르지 않을 경우 ANOVA, Tukey post hoc grouping tests로 검증했습니다.

### 3.2. Results
#### Subjective evaluation and preference
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/173269695-442231ca-22ca-44e1-8281-6a8ed1a8f3ba.png" width="100%"/>
</div>
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/173270256-b593a2db-fda9-4c44-8baf-1038a95c5509.png" width="50%"/>
</div>
> 각 방향에서 가장 선호하는 gestures의 빈도

#### Positional drift
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/173286803-83f5770d-a81f-40f2-9cba-77ac6573473e.png" width="100%"/>
</div>

모든 경우에 평균 위치 편차는 0.03m에서 0.10m 사이였습니다.

TSIP 및 SSIP의 Positional drift가 Rock과 Stay의 Positional drift보다 더 큰 패턴이 발견되었습니다.

### 3.3. Discussion
#### forward walking
walking (0◦)의 경우 TSIP이 subjective evaluation과 overall preference에서 좋은 성능을 보였습니다.

이전 연구에서 SIP는 Tapping-in-place에 비해 신체적 제약이 더 높았던 반면,<br/>
본 연구에선 Exist(Tapping-in-place)가 TSIP에 비해 적합성이 낮습니다. <br/>
또한 피로, 자연스러움에서 유의한 차이가 발견되지 않았으며, 이에 관해 아래와 같은 요소들이 원인임을 추정합니다.
1. 기술 구현의 차이 (본 연구는 Wizard of Oz technique 적용)
2. 움직이는 횟수의 차이 (본 연구는 각 방향당 6~8번 움직임)

따라서, 더 긴 보행에서 Tapping-in-place를 적절하게 구현하면 SIP에 비해 동등한 적합성과 낮은 피로를 유도할 수 있다고 주장할 수 있습니다.

#### sideways and backward walking
sideways, backward walking의 경우는 Rock과 Stay가 ease of use, perceived fatigue 부분에서 좋은 성능을 보였습니다.

Stay는 주관적인 평가들의 결과에 따라 가장 좋은 gestures로 여겨질 수 있으며 편안하고 쉽기때문에, intense shooting, adventure, role-playing, gameplay scenarios와 같은 domain에 적용될 수 있습니다.

반면 Rock은 강한 몰입감을 주는 SSIP와 편안함을 제공하는 Stay의 장점을 갖기에 가장 많은 참가자들이 선호했습니다.

특히 Stay보다 Rock, SSIP가 더 많이 선호된다는 결과로, 이는 실험 1의 결과에서 참가자의 85%가 실제 보행과 유사한 느낌을 얻는 것을 강조한 결과와 반면 15%가 gestures를 유도할 때 편안함에 초점을 둔 결과와 일치합니다.

따라서, 현실감과 생생함이 참가자들이 VR 이동을 위한 gestures를 선택하는 중요한 요소라고 결론지을 수 있습니다.

#### Design implications
- SSIP는 현실감을 극대화하거나 어느 정도의 운동효과를 일으키기에 가장 적합한 선택이 될 것입니다.
- 사용자가 장기간 편안하게 가상 환경을 탐색해야 하는 상황에서는 Stay가 최선의 선택이 될 수 있습니다.
- 일반적으로 Rock은 효율성과 자연스러움 사이의 균형을 유지하면서 최상의 절충안으로 작용할 수 있습니다.
- 또한, 앞으로 걷기를 위한 gestures는 가장 좋은 결과를 보였던 SIP gestures로 교체하는 것이 좋지만 이는 혼란으르 초래할 수 있습니다.

## Limitations and future work
이 연구에는 몇 가지 제한 사항이 있습니다. 
1. 본 연구는 freehand gestures에 대한 문화적 bias가 존재할 수 있습니다. 따라서 한국인이 아닌 사람에게 일반화될지는 의문입니다.
2. Wizard of Oz technique을 기반으로 gestures를 평가했습니다. 따라서 gestures를 식별하는 기술들에 문제가 있을 수 있습니다.

이후의 연구를 통해 표준화된 테스트에서 보다 현실적이고 장기적인 사용 사례에 따라 제안된 gestures를 검증하여 존재 및 멀미를 포함한 VR 관련 측면을 조사할 수 있습니다.

## Conclusion
먼저 사용자로부터 8개의 보행 방향(0◦, ±45◦, ±90◦, ±135◦, 180◦)으로 VR 이동을 위한 WIP gestures를 유도했습니다.
grouping 분석을 기반으로 Turn body + SIP, Step one foot + SIP/Rock/Stay WIP gestures를 선택하고 평가했습니다.

SIP는 직진에 좋은 성능을 보였고, Step one foot + SIP/Rock/Stay의 경우 효율성과 자연스러움 사이의 tradeoff한 다른 보행 방향에 대해 유망했습니다.

이러한 연구 결과는 user로부터 유도된 WIP gestures를 응용하여 VR locomotion에 더 나은 UX와 더 나은 이동 옵션을 제공할 수 있음을 시사했습니다.

---

## 고찰
BCI와 접목한다면 딥러닝과 함께 4방향+sigmoid or 8방향+softmax등을 이용해 Motor Intention 부분을 연구할 수 있을 것 같다.
> 4방향+sigmoid (eg. 상,좌일 확률이 각각 50% 이상일 경우 좌상단으로 이동)

하지만 이 경우, 정면을 향해서만 이동하기 때문에 Motor Imagery(binary classification)를 이용해 얼굴 방향으로 몸을 회전시키는 연구도 고려해볼 수 있을 것 같다.

###### Reference
* DOI: 10.1016/j.ijhcs.2021.102648