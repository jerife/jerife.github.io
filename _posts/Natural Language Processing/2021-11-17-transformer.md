---
layout: post 
title: Introduce to Transformer
subtitle: Transformer with Attention 
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Natural Language Processing]
comments: true
excerpt_separator: <!--break-->
---
<div align=center><h1>Transformer 이해하기</h1></div>
<!--break-->

----

 <br/>
## Transformer란?
기존의 AI의 NLP분야는 RNN계열의 Lstm, GRU, Seq2Seq등의 모델이 사용되었으며, 추가적으로 Attention Mechanism을 추가한 모델을 사용해왔습니다.<br/>
하지만 RNN계열의 고질적인 **"단점"은,**<br/> 
* Sequence한 데이터를 처음부터 끝까지 순차적으로 처리하기에 오랜 시간이 소요된다.
* 데이터의 크기가 큰경우, Backpropagation시 Gradient Vanishing (기울기 소실)이 발생한다. 

위와 같은 단점을 가지고 있습니다. 이런 문제를 해결하기위해 RNN이나 CNN을 사용하지 않고 Sequence한 데이터를 처리할 수 있는 모델인 Transformer가 개발되었습니다.<br/> <br/>
<img width="336" alt="transformer_img_1" src="https://user-images.githubusercontent.com/68190553/142149187-130fa040-eec5-4c89-ac4b-51ab2e9a12f0.png">{: .mx-auto.d-block :} 

Transformer는 Encoder파트 Decoder파트로 구분되며, 언어를 번역해주는 모델이라 가정을하고 설명하겠습니다.

---

### 1) Encoder
RNN계열의 Sequence한 데이터를 처리하는 모델인, Seq2Seq를 기억하실겁니다. Transformer도 Seq2Seq와 마찬가지로 Encoder파트 Decoder파트로 구분되며, 각각은 데이터를 부호화하고 복호화하는 과정을 일컫습니다.<br/> <br/>
하지만 다른점은 RNN을 사용하지 않고, "Positional Encoding"을 이용해 데이터의 위치성을 파악하고 "Scaled Dot-Product Attention"을 이용해 각각의 데이터가 서로 어떤 연관성을 가지고 있는지 파악함으로써 RNN을 대체합니다.<br/> <br/>
또한 Scaled Dot-Product Attention는 어떻게 사용되냐에 따라. "Self Attention" / " Encoder-Decoder Attention"으로 나뉩니다.
* **Self Attention** : 스스로 Attention함으로써, 자기자신과의 연관성을 확인하는 것
* **Encoder-Decoder Attention** : Encoder와 Decoder끼리 Attention함으로써, 집중해야할 것을 확인하는 것

Scaled Dot-Product Attention설명시 이 부분을 잘 숙지하시면서 이해하시길 바랍니다. 

<img width="150" alt="transformer_img_2" src="https://user-images.githubusercontent.com/68190553/142150900-bc211eb6-e783-49de-b5c7-984b0c8efc28.png">{: .mx-auto.d-block :} 

##### [실행 과정]
1. 우리는 번역하고자 하는 문장을 inputs에 넣은 후, 모델이 인식할 수 있게 Embedding해줍니다.
2. Embedding된 데이터(문장)의 위치적인 정보를 얻기 위해 Positional Encoding 값을 구한 후 더해줍니다.
3. 이후 Multi-Head Attention(여러개의 Scaled Dot-Product Attention을 의미함)를 진행합니다.
4. Multi-Head Attention을 진행한 값과 진행하기 이전 값을 Residual connection(잔여학습: Add)해준 후, Normalization(정규화: Norm)까지 적용해줍니다.
5. CNN모델에 등장하는 Fully Connected를 진행해줍니다. (출력을 필요한 형태로 변형함) 
6. Fully Connected Layer를 지난 값과 이전 값을 Residual connection(잔여학습: Add)해준 후, Normalization(정규화: Norm)까지 적용해줍니다. <br/> <br/>

#### [1] Positional Encoding
![transformer_img_4](https://user-images.githubusercontent.com/68190553/142158745-4dc1d5e0-3bcf-4d57-a24c-63bb753d21df.png){: .mx-auto.d-block :}

우리는 (실행과정-1)과 같이 inputs(문장)을 Embedding해줍니다. 논문에선 Embedding의 Dimension을 512로 해줬지만, 위 이미지는 Dimension은 4입니다.<br/> <br/>
우리는 RNN을 사용하지 않기 때문에, Sequence적인(시간의 흐름같은) 특징을 알지 못합니다. 그래서 단어의 위치를 의미해주는 값인 Positional Encoding을 더해줍니다.(실행과정-2)
![transformer_img_6](https://user-images.githubusercontent.com/68190553/142161556-a9ec0cbd-7e9d-4fa4-ac2c-49e2214a65af.png){: .mx-auto.d-block :}
<img width="243" alt="transformer_img_5" src="https://user-images.githubusercontent.com/68190553/142160223-346657fb-f76b-4486-be5d-83e1279a7634.png">{: .mx-auto.d-block :}
논문에선 Sequence적인 위치를 표현하기 위해 sin/cos 함수를 이용해 값을 표현했습니다.<br/>
Positional Encoding을 sin/cos 함수를 사용하는데 있어 "장점"은,
* 1. 위 함수는 [-1,1]사이의 값을 갖고있어 정규화되어 있다.
* 2. 연속적인 함수이기에 아무리 긴 input도 Sequence적인 위치를 표현할수있다.
    
와 같은 장점이 있기에, 논문에선 위와 같은 Positional Encoding을 이용했습니다.

#### [2] Multi-Head Attention
<img width="452" alt="transformer_img_7" src="https://user-images.githubusercontent.com/68190553/142338729-42bddaf4-1315-4330-b07c-c2189d217075.png">{: .mx-auto.d-block :}
(실행과정-3)에 해당하는 Multi-Head Attention은 "Scaled Dot-Product Attention" 여러개를 병렬처리한 부분입니다. 논문에선 8개의 Self Attention을 한번에 처리했습니다.<br/>
그전에 Scaled Dot-Product Attention에 대해 먼저 알아보도록하죠. (Mask는 제외하고 진행하겠습니다.)

![transformer_img_11](https://user-images.githubusercontent.com/68190553/142338994-0a819f40-4fa4-430d-853e-d4c7563a9b40.png){: .mx-auto.d-block :} 
위사진 속엔 "Thinking Machines"라는 문장을 Embedding을 하고, Positional Encoding이 더해진 초록색 2x4벡터가 있습니다.<br/> <br/>
Scaled Dot-Product Attention에선 2x4벡터를 Q(Query), K(Key), V(Value)로 분해시킵니다. 분해하는 과정에서 우리는 W^Q, W^K, W^v 를 Dot product해줌으로써 Q, K, V를 얻습니다.
> Q, K, V 각각의 단어가 서로 어떤 연관성을 보이는지 구할수 있게 해주는 값입니다.

![transformer_img_12](https://user-images.githubusercontent.com/68190553/142339536-8ad3d7f3-870f-425e-9e1b-2fa17c5daabd.png){: .mx-auto.d-block :}
<img width="242" alt="transformer_img_16" src="https://user-images.githubusercontent.com/68190553/142353510-acbe492c-069f-4f91-9a2a-9423aa4c1609.png">{: .mx-auto.d-block :}
그 후, 위와 같은 연산을 해줌으로써 Q의 각 단어가 K에 얼마나 영향이 있는지 확인하고(Dot product), 이를 정규화해준 후 확률값(softmax(dim=1))을 취해줍니다. (이 과정까진 2개의 스칼라 값을 가지게 됩니다.)<br/> <br/>
이후 위에서 얻은 스칼라값과 V를 곱(dim=1)해줍니다.

![transformer_img_15](https://user-images.githubusercontent.com/68190553/142340693-cbcdea3a-5a5e-4f7e-be88-355e21a95175.png){: .mx-auto.d-block :} 
위의 내용을 정리하면, 이 사진과 같은 과정이 되는것이죠.<br/> <br/>
밑에선부턴 이 과정을 동시에 여러번 동작되게, 즉 여러개의 Scaled Dot-Product Attention을 병렬처리하는 Multi-Head Attention을 알아보도록하겠습니다.

![transformer_img_14](https://user-images.githubusercontent.com/68190553/142340966-fd708d41-e200-45b5-b2bb-b4cede8d455b.png){: .mx-auto.d-block :} 
위 사진속에선 W0 ~ W7까지 총 8개의 Scaled Dot-Product Attention을 병렬처리하는 내용을 보여줍니다. 결국 Z0~Z7의 갯수도 8개가 되는걸 알 수 있습니다.<br/> <br/>
이제 이 Z를 Concatnation(dim=1)해준 후, W^O의 가중치와 합해줘서 최종 Z(맨 오른쪽)의 값을 얻습니다. 여기서 최종 Z의 크기는 입력값으로 넣은 X(2x4벡터)와 "같은 크기"임을 알 수 있죠.

{: .box-warning} 
"Multi-Head Attention혹은 에 값을 넣으면, 입력값과 출력값의 크기가 같다."


입력값과 출력값의 크기가 같다는 것은 Multi-Head Attention을 직렬화 처리해서 여러번 붙여넣을 수도 있다는 의미입니다. 실제로 논문에서도 Encoder을 7번 직렬화시켰습니다.<br/> <br/>

#### [3] Add&Norm
이제 (실행과정-4/6)에 대해 설명하겠습니다. 이과정에선, Multi-Head Attention/Feed Forward를 거치기 "이전 값"과 "이후 값"을 서로 Residual connection(더해줌)를 진행해줍니다.<br/> <br/>
"이전 값"과 "이후 값"은 서로 크기가 같아 Residual connection(더해줌)이 가능합니다! 추가적으로 Normalization(정규화: Norm)을 해줍니다.<br/> <br/>

#### [4] Feed Forward
이는 우리가 흔히 알고있는 Fully Connected를 의미하며, Feed Forward층을 늘려줌으로써 연산과정을 한번 추가합니다. <br/> <br/>

### 2) Decoder
<img width="150" alt="transformer_img_3" src="https://user-images.githubusercontent.com/68190553/142353216-e47949d9-0439-4bc4-bf5f-0de9f31ddf10.png">{: .mx-auto.d-block :} 

Decoder는 위에서 다룬 Encoder와 비슷한 구조를 갖지만, Multi-Head Attention을 두번 진행한다는 차이점을 위조로 설명해보겠습니다.<br/> <br/>

#### [1] Masked Multi-Head Attention
<img width="452" alt="transformer_img_7" src="https://user-images.githubusercontent.com/68190553/142353716-0614e087-80ee-4c27-a49f-3fddafda7bcc.png">{: .mx-auto.d-block :} 

먼저 Encoder에서 언급한 Multi-Head Attention 구조를 다시 살펴봅시다. Encoder서의 Scaled Dot-Product Attention(Self Attention)을 할때는 Mask(opt.)을 제외하고 진행했습니다.<br/> <br/>
밑에서 Mask(opt.)에 관한 언급을 하겠습니다.

![transformer_img_17](https://user-images.githubusercontent.com/68190553/142354221-023c01bb-47d8-4e5f-bae1-ef535271feab.png){: .mx-auto.d-block :} 
Mask는 디코더 과정에서 target 단어 이후의 단어를 보지 않고 예측하기 위해, 마스킹해주는 작업입니다.<br/> <br/>
##### [실행 과정]
1. Q(Query)와 K(Key)를 Dot product해주고 정규화한 이후에, Mask를 적용해줍니다. Mask가 필요한 부분에 -♾️를 대입해준다.
2. 그리고 Softmax를 취해준다.

> -♾️ 들어간 곳은 이후 Softmax를 취해질 경우 0의 값을 가집니다.

{: .box-warning}
"Mask를 취해줌으로써 다음 단어를 미리 알지 못한다."

 <br/>
#### [2] Encoder-Decoder Multi-Head Attention
Decoder의 두번째 Multi-Head Attention 부분을 보면,  1개 값은(Q(Query) Masked Multi-Head Attention에서 가져오고,  2개의 값(K(Key), V(Value)은 Encoder로부터 받는 것을 알 수 있습니다.<br/> <br/>
**왜 Q(query)를 Decoder에서 받아올까요?**<br/> <br/>
Q(query)는 조건에 해당하기 때문입니다. Decoder에서 나온 결과중 어떤것에 집중해야할지를 Encoder를 통해 알게됩니다. <br/>
Q(query)는 이미  Masked Multi-Head Attention에서 masking 됐으므로, i번째 위치까지만 Attention을 받습니다.<br/> <br/>
Decoder의 이 두가지 Attetion과정 이외에는 Encoder와 같으므로 설명을 생략하겠습니다.

---
### Performance
![transformer_img_18](https://user-images.githubusercontent.com/68190553/142361266-2d27964f-b2ac-4479-a07a-72ba31ec2b3b.png){: .mx-auto.d-block :}
> 특정 Encoder/Decoder Layer의 Attention의 해석을 시각화할 수 있음

Transformer(어텐션 메커니즘)의 유용한 속성은 해석이 가능하며, 각 단어가 서로에게 어떤 연관성을 주는지 시각화해 확인할 수 있습니다.<br/> <br/>
Encoder Layer의 Attention을 시각화 한 자료를 마지막으로 보여드리면서, Transformer에 대한 설명을 마치겠습니다.<br/> <br/>

###### Reference
*  https://arxiv.org/abs/1706.03762
*  https://jalammar.github.io/illustrated-transformer/