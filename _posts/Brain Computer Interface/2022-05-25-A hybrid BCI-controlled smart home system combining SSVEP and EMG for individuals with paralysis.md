---
layout: post 
title: Review - A hybrid BCI-controlled smart home system combining SSVEP and EMG for individuals with paralysis
subtitle: hybrid BCI-controlled SSVEP and EMG
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Brain Computer Interface]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2>A hybrid BCI-controlled smart home system combining SSVEP and EMG 논문 리뷰</h2></div>
<!--break-->

----


 <br/>

## ABSTRACT
본 논문에선 EMG(electromyogram)와 SSVEPs(steady-state visual evoked potentials)를 결합해 마비환자를 위한 hybrid brain–computer interface(hBCI) 기반 스마트홈 제어법을 제안합니다.

Participants는 화면속에서 서로 다른 frequency로 반짝이는 stumli를 응시하면서 조작합니다. 이후 SSVEPs의 의도를 식별하기 위해 4개의 EEG에 correlation analysis(CCA)가 사용됩니다.

selected function, return from the sub-interface to the main interface, switch the system on/off 하기위해 관자근에서 발생하는 occlusal EMG를 이용합니다. 

건강한 참가자와 마비 환자가 평균 97.5%, 83.6%의 정확도를 달성했지만 EMG와 SSVEPs를 결합한 결과 정확도가 100%로 극대화되었습니다. 또한 이 task는 높은 workload를 요구하지 않아서 효과적이고 안전하다고 합니다.

## INTRODUCTION
EEG-based BCI는 안전하고 낮은 cost에 접근성이 쉽다는 이유로 많이 활용되고 있습니다.

특히 SSVEP-based BCI는 training과정이 필요 없고 information transmission rate (ITR)이 빠르다는 장점이 있기 때문에 휠체어 제어, 병원 침대제어 등에 사용되곤 했습니다.

하지만 대부분의 single EEG는 single device만 control할 수 있으며 single EEG경우 보통 Synchronous하고 지속적으로 일정한 자극 필요합니다. 따라서 유저의 피로도가 올라가거나 정확도가 감소하는 일이 발생합니다.

마비환자들은 인지를 잘 못하거나 뇌파가 비정상적으로 작동하는 경우도 있기 때문에 마비환자를 대상으로 하는 연구는 단순함이 필요합니다.<br/>
추가적으로 대부분의 마비환자는 얼굴 근육의 신호만을 사용할 수 있기 때문에 단순함뿐아니라 적은 workload를 요구해야합니다.

본 논문에선 4-Channel EEG로 환자가 의도하는 coomand를 식별합니다.
그리고 1-Channel EMG로 target을 확인하고, 메인메뉴로 돌아가고, 시스템을 on/off합니다.

## METHODOLOGY
### A. Smart home system

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170159914-b72f77e5-7b45-4e2e-8382-b89640f5399a.png" width="100%"/>
</div>

hBCI 시스템은 두가지 모듈로 구성됩니다.
1. EEG/EMG fusion module
2. smart home control module

fusion module은 visual stimulation interface, signal acquisition/processing, feature classification, command coding, feedback systems의 역할을 합니다.

또한 laptop LCD monitor를 통해 stimuli되고 TCP/IP로 working computer에서 연산합니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170161381-aa902a4d-5713-4927-b006-fbd520bee4c5.png" width="100%"/>
</div>

control module의 경우 
fusion module에서 얻어진 결과를 바탕으로 continuous voltage signals, switch signals, digital signal로 다른 device를 제어합니다.
> eg. 휠체어의 경우, switch signals을 사용하여 휠체어의 위치를 변경하고 continuous voltage signals을 사용하여 연속적인 이동 및 회전합니다.

### B. Functional interface
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170161594-61446a9c-803e-4cba-a84d-d6170f016cef.png" width="100%"/>
</div>

interface는 아래와 같이 구성되어있습니다.
1. idle interface
2. main interface
3. 5-sub interface (wheelchair, hospitalbed, television, telephone, curtain/lights)

각 interface는 일치하는 frequency로 flickering(stimuli)합니다. user는 target을 응시함으로써 적절한 장치와 기능을 선택할 수 있습니다. 명령이 확인되면 flickering(stimuli)은 녹색으로 표시되고 선택한 기능이 표시됩니다.

### C. Signal processing
#### SSVEP detection
수집된 EEG의 baseline은 average values를 사용해 제거됐습니다. 이후 low / high frequency band noise를 제거하기위해 3-40Hz BPF가 사용됐습니다.

이후 SSVEP frequency를 식별하기위해 Classical correlation analysis (CCA)를 사용했습니다. 

{: .box-warning}
CCA란?<br/>
두 다차원 변수 집합 사이의 근본적인 상관 관계를 밝히는 방법이며, 변환된 두 집합이 최대 상관 관계를 갖도록 두 집합에 대한 Linear transform 쌍을 찾습니다.

인터페이스에 해당하는 flickering(stimuli)은 서로다른 frequency를 갖습니다.
> eg. target이 5개면, frequency는 각각 8,9,10,11,12 Hz

EEG는 후두부의 PO4, PO3, O1, O2 channel에서 사용됩니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170181902-8756cfec-94cc-4a7b-a63e-15ce42bb7f8a.png" width="40%"/>
</div>
> sample rate : fs<br/>
the number of target : n <br/>
the number of harmonics : h

이후 기준되는 신호 $Y_{fn}$은 flickering(stimuli)로 얻어진 frequency의 sin/cos파와 이것의 매초 마다의 고조파으로 구성되어있습니다.

#### EMG detection
EMG signal은 관자근에서 얻어졌으며, threshold algorithm에 기반한 integral EMG (iEMG)으로 분류되었습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170185722-08b1faef-2ad2-45ce-911f-177812e1dbe7.png" width="40%"/>
</div>
> Envelope (n)은 iEMG의 n번째 point <br/>
EMG(i)는 EMG data의 i번째 sample point<br/>
W(window size) = 20

먼저 EMG data는 20–150 Hz BPF를 거친 후 iEMG을 이용해 envelope of EMG data가 추출됩니다.

1000 point의 EMG data는 window로 인해 50 segment가 됩니다. 따라서 iEMG도 50 segment를 얻으며, envelope of EMG data를 이용해 EMG patterns을 탐지할 수 있습니다.

이후 mean filter를 이용해 envelope curve를 스무딩했으며, clench 주기는 임계값을 초과하는 point의 수를 기반으로 계산되었습니다.
> clench : 턱에 힘을 줘 치아르 깨무는 현상

clench 지속시간이 지정된 범위 내에 있는 경우 아래 3가지 Pattern을 갖습니다.

- EMG Pattern 1 : 0.2~1.0초 동안 1번 clench 지속되는 경우
- EMG Pattern 2 : 0.2-1.0초 동안 1번 clench 지속되며, 1초 이내에 다른 클렌치가 있는 경우
- EMG Pattern 3 : 2.0초 이상 clench 지속됩니다.

#### Fusion protocols

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170189350-3a5778c5-5b60-4418-8ed9-1ad2037a4f36.png" width="80%"/>
</div>

control mode는 EMG patterns과 SSVEPs를 사용해 개발됐으며, 크게 3가지 명령이 가능합니다.
- Switch command: state를 idle과 working으로 변환해줌 [EMG Pattern 3]
- Return command: device interface를 main interface로 변환해줌 [EMG Pattern 2]
- Selection & Confirmation command: user가 희망하는 control을 선택해줌 [SSVEPs & EMG Pattern 1]

최종적으로 본 논문에서 제시한 hybrid smart home system 동작과정을 알아보겠습니다.

**[동작과정]**
1. idle state에서 EMG Pattern 3를 이용해 system을 작동시킨다.
2. main interface에서 sub interfaces로 넘어가기위해 2가지 과정이 필요합니다. 
3. device selection stage에서 원하는 interface의 flicker(stimuli)를 응시합니다.
4. device confirmation stage에서 응시했던 flicker(stimuli)는 SSVEP 특성을 통해 녹색으로 표시되고 EMG Pattern 1을 이용해 sub interface로 넘어갑니다.
5. 위의 [3][4]과정과 같이 Device functions stage에서 SSVEP로 인식하고 EMG로 확인합니다.

- EMG Pattern 2는 sub interfaces에서 main interface로 넘어가는 역할을 합니다.
- Device를 사용할때 wheelchair / curtains 의 경우 EMG Pattern 1을 이용해 멈춥니다.

## EXPERIMENT
### A. Online verification
EEG와 EMG data는 데이터는 1000$F_{s}$로 수집되었습니다.
EEG는 후두부의 PO4, PO3, O1, O2 channel에서 수집되었으며, EMG의 경우 관자근 밑 부분에서 수집되었습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170451066-e34248b0-ffb2-41b7-ac8e-61b7de738bc5.png" width="50%"/>
</div>
> screen refresh rate : F
> i 번째 프레임의 stimulus frequency : f

Subject는 의자에 앉아서 70cm 앞의 LCD 모니터를 응시합니다.
flicker(stimuli)는 사인파를 이용해 구현되었습니다.

실험은 37 target Selection & Confirmation commands, 5 Return commands, 2 Switch commands를 포함합니다.

### B. Evaluation
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170451489-6a922ca8-9f4c-47c9-aaf8-602b4b35c774.png" width="65%"/>
</div>
Information Transfer Rate (ITR)는 위의 식으로 계산되었으며, ITR이 높다는 것은 시간당 더 많은 정보를 전달할 수 있다는 것을 의미합니다.

NASA-TXL를 이용해 Subject의 workload를 측정했습니다.
NASA-TXL는 mental demand, physical demand, temporal demand, effort level, performance, frustration levels 과 같은 평가척도를 지닙니다.


## RESULTS
### A. Control accuracy
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170452631-01302ede-b4cb-4ba2-b158-9fabaf97508f.png" width="100%"/>
</div>

위 table을 보면 Selection 이후 Confirmation을 하는 과정은(Control 과정) 보통사람, 마비환자 둘다 100% accuracy를 기록합니다.

### B. Information transmission rate (ITR)
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170453221-175ed923-e0d6-4aa4-8a83-c8333d909cb5.png" width="90%"/>
</div>
위는 confirmation 과정을 추가하거나 빼고 ITR을 측정한 결과입니다.

confirmation 과정은 잘못 처리된 값은 취소할 수 있으며 100%의 accuracy를 보장할 수 있지만 3~4초 정도의 시간이 더 걸립니다.

### C. Brain workload
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170453247-a12557f9-31b5-4d5e-ae1b-719293e1fc33.png" width="90%"/>
</div>
NASA-TXL의 6가지 척도 (mental demand, physical demand, temporal demand, effort level, performance, frustration levels)은 위와 같은 결과를 보입니다.

frustration, performance, effort의 결과를 보면 마비환자도 논문에서 제안된 시스템에 만족감을 느끼는 것이라 합니다.

## DISCUSSION & CONCLUSION
occlusal의 EMG와 SSVEP를 결합한 hBCI-based smarthome control system은 정확할뿐아니라 안전하고 효울적입니다.<br/>
또한 ITR도 좋고 다른 BCI-basedhome control system보다 false positives의 비율이 낮으며 mental, physical workload가 낮다는 장점이 있습니다.
그리고 EMG를 통해 interface간 이동도 풍부하게 제공합니다.


이후엔 tree-like GUI나 다양한 device, target function을 적용할 것을 제안합니다.<br/>
또한 기능적인 부분에서 occlusion 패턴인식을 이용한 return, switch command가 약한 clench EMG 때문에 정확도가 떨어지기에 향상시킬 것을 제안합니다.<br/>
또한 64-channel EEG cap은 비쌀뿐아니라 번거롭기 때문에 마비환자가 착용할 수 있는 signal acquisition 장비에 대한 연구도 제안합니다.

---

## 고찰
'완전몰입가상현실' 구현만을 위해 VR과 BCI를 접목시키려 했지만 EMG 같은 신체 신호를 이용해서도 hand free한 VR환경을 구현 할 수도 있을것 같다는 생각이 들었다.

###### Reference
* https://www.sciencedirect.com/science/article/pii/S174680941930268X