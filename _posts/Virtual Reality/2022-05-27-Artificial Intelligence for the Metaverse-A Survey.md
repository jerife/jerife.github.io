---
layout: post 
title: Review - Artificial Intelligence for the Metaverse - A Survey
subtitle: Artificial Intelligence for the Metaverse
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Virtual Reality]
comments: true
use_math: true
excerpt_separator: <!--break-->
---
<div align=center><h2>Artificial Intelligence for the Metaverse - A Survey 논문 리뷰</h2></div>
<!--break-->

----


 <br/>

## ABSTRACT
본 논문에선 메타버스 기반의 AI 역할을 탐구하며, 크게 6가지로 나눠집니다.
- natural language processing
- machine vision
- blockchain
- networking
- digital twin
- neural interface

또한 메타버스(virtual world)를 기반한 이 기술은 아래와 같은 분야에 적용될 수 있습니다.
- healthcare
- manufacturing
- smart cities
- gaming

## INTRODUCTION
머지 않은 미래에 메타버스는 차세대 기술로 구현되어 게임 제작자, 인터넷 금융 기업, 소셜 네트워크 및 기타 기술 리더를 끌어들이고 있습니다.


<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170640351-d97508d3-7456-4c02-8cad-00acbd0cc7fe.png" width="80%"/>
</div>

온라인 게임 사업은 세계의 수입의 반 이상을 차지할 것이며, 특히 virtual reality(VR) 하드웨어 및 게임 내 광고의 수익은 메타버스의 가상 활동의 발전을 통해 크게 증할 것으로 예측됩니다.


<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170640726-7e34063f-7fde-4de8-91de-35a607cbe7bf.png" width="100%"/>
</div>
> 메타버스 발전에 기여한 큰 이벤트의 timeline

현재 메타버스는 공유된 가상 3D 세계 또는 사용자에게 상호작용 및 협업 활동으로 포괄적인 몰입 경험을 제공할 수 있는 "여러" 플랫폼 간 세계로 정의됩니다.


기존의 메타버스는 부족한 데이터로 인해 몰입적이지 못한 경험을 제공했습니다. 

하지만 최근 몇년간 GPU와 같은 하드웨어의 발전으로 인한 3D게임에서 파생된 메타버스의 폭발이 목격되었습니다.<br/>
또한 software의 최적화로 인해 가상세게를 반드는 것은 더욱 견고하고 창의적으로 구축되어집니다.

또한 대량의 데이터를 만들어내기 떄문에 AI의 적용까지 더욱 용이합니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170642649-db9977b6-8384-4b5b-9189-b5ae7061fa77.png" width="50%"/>
</div>
메타버스 플랫폼은 위와 같은 7개의 layer를 포함할 수 있습니다.

- Infrastructure(인프라): 5G, 6G, WiFi, cloud, data center, central processing units, and GPUs.
- Human interface(인간 상호작용 인터페이스): mobile, smartwatch, smartglasses, wearable devices, head-mounted display, gestures, voice and electrode bundle.
- Decentralization(탈 중앙화): edge computing, AI agents, blockchain, and microservices.
- Spatial computing(공간 컴퓨팅): 3D engines, VR, augmented reality (AR), XR, geospatial mapping, and multitasking.
- Creator economy(창조 경제성): design tools, asset markets, Ecommerce, and workflow.
- Discovery(발견): advertising networks, virtual stores, social curation, ratings, avatar, and chatbot.
- Experience(경험): games, social, E-sports, shopping, festivals, events, learning, and working.

machine learning(ML)과 deep learning(DL)의 측면에서 위 7개 layers 내 AI의 존재를 발견하는 것은 어렵지 않습니다.

## AI FOR THE METAVERSE: PRELIMINARIES
### A. Categorization of AI
간략하게 machine learning(ML) / deep learning(DL)을 크게 2가지 범주로 짚고 넘어갑니다.

1. Conventional Techniques: Conventional AI/ML algorithms
은 supervised learning, unsupervised learning, semi-supervised learning, and reinforcement learning 나뉘어집니다.

2. Advanced Techniques: deep learning(DL)은 multi-layered artificial neural networks 을 적용해 다양한 classification, regression task에서 state-of-the-art를 달성합니다. deep learning(DL)은 다양한 domian의 applications에 적용되곤 합니다.

### B. Role of AI in the Metaverse
메타버스 플랫폼의 7개의 layer에 따르면, 지금까지의 인프라의 신뢰성을 보장하고 성능을 향상시키는 AI의 중요한 역할을 실현하는 것은 의심할 여지가 없습니다.

아래와 같은 것들은 machine learning(ML) / deep learning(DL)에 의해 분석되고 식별될 수 있습니다.
- facial expressions
- emotions
- body movement
- physical interactions
- speech

때문에 아래와 같은 것들에 적용될 수 있습니다.
- sensor-based wearable devices
- human-machine interaction gadgets
- simple human movements
- complex actions 


## AI FOR THE METAVERSE: TECHNICAL ASPECT

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170648124-1996bba0-502a-4475-9349-68701996a52c.png" width="100%"/>
</div>
- natural language processing
- machine vision
- blockchain
- networking
- Digital Twin
- neural interface

### A. Natural Language Processing
자연어 처리(NPL)는 컴퓨터 언어학이라고도 하며, 음성 및 텍스트를 포함한 인간의 언어를 자동으로 분석하고 이해하는 실질적인 문제를 해결하기 위해 다양한 계산 모델과 학습 과정을 포함합니다.

NPL에선 주로 speech-to-text, text-to-speech, conversation design, voice branding, multi-language, multicultural in voice와 같은 task들을 다룹니다.

NLP는 가상 환경에서 사용자를 지원하기 위한 지능형 가상 비서(챗봇)와 관련하여 메타버스에서 중요한 역할을 합니다

최근엔 attention mechanism, bi-directional structure,  문장과 짧은 문단의 장기적인 의존성을 다루기 위한 CNN등의 기법들이 제안되고 있습니다.

### B. Machine Vision
컴퓨터 비전과 XR을 포함한 Machine Vision은 메타버스의 토대를 얻기 위한 핵심 기술 중 하나입니다.

Machine Vision을 통해 사용자는 가상 세계의 아바타를 3D 맵에서 자유롭게 이동하고 메타버스의 가상 객체와 상호 작용할 수 있습니다.

#### 1) Extended Reality
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170820273-d8717bde-c2e8-4bda-836e-849996dc0818.png" width="100%"/>
</div>
AR은 물리적 세계에서 그래픽, 비디오 스트림 및 홀로그램의 경험을 제공하고 VR은 완전한 몰입형 디지털 세계에서 시청 경험을 제공하는 반면, MR은 AR과 VR 사이의 전환 경험을 제공할 수 있습니다. 이러한 현실 기술과 함께, 인간 사용자들은 메타버스를 경험할 수 있고 다양한 서비스를 즐길 수 있습니다.

XR과 AI는 별개의 부문이지만, 메타버스에서 완전히 몰입할 수 있도록 결합할 수 있습니다. 

시각 기반 정보를 기반으로 인간-기계 경험을 개선하기 위해 VR 기기에 적용되었습니다.
- AI를 이용한 eye tracking HMD
- 제스처의 다차원 움직임을 인식하기 위한 센서장갑

미래에는 메타버스에서 사용자의 시각적, 대화적 경험을 향상시키기 위한 새로운 장치가 홀로그램 장치와 몰입형 장치 사이의 차이를 최소화해야한다 합니다.

#### 2) Computer vision
몇몇 기본적인 Computer vision 기술은 메타버스에서 인간 사용자들의 경험을 향상시킬 수 있는 잠재력이 있습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170820826-4765a30b-be10-44a7-acb9-62b30b383d35.png" width="100%"/>
</div>

Computer vision의 가장 기본적인 기술은 segmentation, object detection이 있습니다.

- segmentation은 이미지의 각 픽셀을 분류합니다.
- object detection은 이미지에 경계 상자를 그려 탐자하는 것을 목표로 합니다.

최근엔 첨단 영상 처리 및 깊이 감지 알고리즘을 사용해 3D object detection분야가 발전 중에 있습니다.

그 밖에도 가상 세계에서는 사용자의 시각적 인식을 풍부하게 하기 위해 노이즈, 흐림 및 낮은 해상도와 같은 일부 이미지 품질 감소 문제를 해결해야 합니다. 

- 색 왜곡을 피하면서 feature map을 개선하기 위해 residual 구조와 U-Net을 결합하는 기법
- CNN을 활용하여 이미지 압축 artifact를 줄이고, 축소되고 흐릿한 이미지를 복원하고, 누락된 정보를 재구성하는 기법

와 같은 기법들이 존재합니다.

또한 사용자의 행동이나 움직임을 추적하기 위해 손 제스처 인식, 보행 식별 및 시선 추적, 관절 추적등의 기술은 XR 환경에서 대화형 경험을 향상시킨다고 합니다.


### C. Blockchain
블록체인은 권한이 있는 네트워크 구성원만 접근할 수 있는 변경 불가능하고 침투할 수 없는 곳(ledger: 원장)에 저장된 즉각적이고 공유되며 투명한 정보를 제공할 수 있습니다.

메타버스에서 대량의 데이터(eg. 비디오 및 기타 디지털 콘텐츠)가 VR 장치에 의해 획득되어 네트워크를 통해 전송되고 어떠한 보안 및 개인 정보 보호 메커니즘 없이 데이터 센터에 저장되므로 사이버 공격의 민감한 대상이 될 수 있습니다. 이러한 맥락에서, 몇 가지 고유한 기능을 가진 블록체인은 특히 AI 기술로 강화될 때 메타버스의 보안 및 개인 정보 보호 문제에 대한 유망한 솔루션을 드러냅니다.

또한, 서비스가 사용자에게 제공하는 많은 활동과 이벤트는 블록체인을 통해 투명하게 거래되고 기록되어 수많은 메타버스 내 디지털 자산을 추적하고 산출할 것입니다.

블록체인 기반 네트워크에서 AI는 사이버 공격을 탐지하고 분류하기 위한 데이터 해석 및 이상 예측이 사용된다고 합니다.

최근 Federated learning (FL)은 여러 사용자가 자신의 로컬 데이터로 AI 모델을 학습하고 parameter aggregation mechanism에 의해 서버에서 글로벌 모델을 협업적으로 학습하는 데이터 공유의 개인 정보 보호 문제를 해결하는 효과적인 솔루션이 떠오르고 있습니다.

#### D. Networking
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170902543-fa276f20-4be7-4e0f-9bca-f6fa64f54a91.png" width="70%"/>
</div>

메타버스는 무선 네트워크로 수많은 사용자에게 서비스를 제공합니다.

메타버스의 실시간 멀티미디어 서비스 및 애플리케이션은 일반적으로 최소 기본 수준의 사용자 경험을 보장하기 위해 높은 처리량과 짧은 대기 시간으로 안정적인 연결을 요구합니다.

ML/DL은 5G/6G 네트워크에서 지능형 무선 리소스 할당과 같은 기존의 까다로운 작업을 효과적으로 처리하는 동시에 매우 낮은 대기 시간을 충족할 수 있는 큰 잠재력을 보여주었습니다. RL은 향상된 모바일 광대역(eMBB) 및 초신뢰성 및 저지연 통신(uRLLC)에 대한 리소스 슬라이싱 문제를 해결하기 위해 활용되었습니다.

#### E. Digital Twins
DT는 구조 및 기능을 포함한 현실의 정확한 복제를 만들어 사용자가 가상 세계에 들어가 서비스를 즐길 수 있는 게이트웨이 역할을 합니다.

개발자와 서비스 제공업체는 DT와 AI를 통해 원격으로 모든 종류의 물리적 분석을 수행할 수 있는 머신과 프로세스를 가상 복제할 수 있습니다.

<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170916283-2cb1684f-dcd9-41fb-931b-dbe68baed51b.png" width="60%"/>
</div>

예시로 건강 진단 성능을 개선하고 지능형 의료 시스템에서 더 나은 건강 운영을 촉진하기 위해 데이터 기반 DT가 연구되었습니다.<br/>
DT는 건강 프로필의 가상 복제를 만들고, 의료 전문가의 협업 활동을 수행하고, 동일한 경우 환자를 위한 보편적 치료 계획을 수립하는 데 서로 다른 단계에 기여했습니다.<br/>
ML 모델은 의료 사물 인터넷(IoMT) 장치가 수집한 원시 데이터에서 의미 있는 정보를 학습하여 건강 이상을 조기에 발견하고 건강 문제를 정확하게 인식하도록 구축되었습니다. 



#### E. Neural Interface
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170902887-8790f7f1-a5ca-4e43-84dc-bfa09ad13446.png" width="60%"/>
</div>
가상 작업과 상호 작용할 수 있는 가장 몰입적인 인터페이스는 VR 헤드셋입니다. BCI/BMI는 인간과 웨어러블 기기 사이의 경계를 명확히합니다.

패턴 인식 단계의 AI/ML 알고리즘을 통해 복잡하고 민감한 신경 신호를 정확하게 분석할 수 있습니다.

Neural Interface는 크게 두가지 접근법이 있습니다.
1. 신호에 라벨을 부여하고 모델을 학습하고 분류하는 기법 (eg. motor imagery, mental analysis event-related potential)
2. 신호를 기준에 따라 분류하는 기법

AI는 주로 1번 task에서 사용됩니다.<br/>
기존엔 ML(SVM/Linear Regressor)로 분류했지만 이후 common spatial pattern algorithm이 적용되어 정확도가 더욱 향상되고 현재는 DL로 분류하는 추세라고합니다.

현재 CNN / LSTM등 다양한 기법들이 적용되어 연구되어지고 있으며 특징 추출 및 특징 설명 측면에서 capsule network (CapsNet)이 사용된다고도 합니다.

## AI FOR THE METAVERSE: APPLICATION ASPECT
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170905750-ae4350bf-6921-44c3-9995-38cf4f662a9e.png" width="100%"/>
</div>

위에서 언급되어진 메타버스 기술들은 아래와 같은 domain에서 적용되어질 수 있습니다.
- healthcare
- manufacturing
- smart cities
- gaming

논문에선 위 사진처럼 각 산업별로 AI가 활용되는 부분을 언급합니다.


## METAVERSE PROJECTS
<div align="center">
    <img src="https://user-images.githubusercontent.com/68190553/170905750-ae4350bf-6921-44c3-9995-38cf4f662a9e.png" width="100%"/>
</div>

해당 파트에선 현재의 메타버스들을 소개합니다. 본 포스팅에선 생략하도록 하겠습니다.

- Decentraland
- Sandbox
- Realy
- Star Atlas
- Bit.Country
- DeHealth

## CONCLUSION AND RESEARCH DIRECTIONS
본 논문은 메타버스 기반에서 AI의 역할과 가상 세계에서 사용자 몰입 경험을 향상시킬 수 있는 잠재력을 종합적으로 조사했습니다.

현재의 메타버스는 (eg. Decentraland, Sandbox, etc) 유저의 경험, 소유권, 가상세계를 커스텀하는 것이 부족합니다. 미래에는 AI가 초현실적인 물체와 더 쉽고 빠르게 컨텐츠를 접할 수 있게 해줄것이라 합니다.

미래에 AI가 메타버스와 더 직관적이고, 보안에 안전하고, 법적으로 문제가 생기지 않기 위해선 AI의 단점인 Blackbox가 해결되어야한다고 합니다. 
따라서 XAI가 AI들을 설명해주는 것이 중요하다고 말하며 마무리됩니다.
> Blackbox: AI가 예측할떄 어떤 근거를 가지고 결과를 도출한지 알수 없어서 "AI는 블랙박스다" 라고함.

---

## 고찰
최근 메타버스, 코인, NFT, 블록체인, VR등의 기술들이 각광 받는 이유는 모두 상관성이 있기 때문임을 알 수 있었다.

게다가 AI기술이 대부분의 산업에 적용되고 있음을 보고 각 domain에 대한 지식이 크게 중요하다는 것을 느꼈으며, BCI와 VR 접목에 관심이 있는 나는 더욱 Brain / Game 쪽 도메인에 능숙해야 필요가 있다는 생각이 들었다.

###### Reference
*  https://arxiv.org/abs/2202.10336