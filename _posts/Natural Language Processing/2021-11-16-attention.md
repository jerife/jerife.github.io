---
layout: post 
title: Introduce to Attention Mechanism
subtitle: Attention Is All You Need 
gh-repo: jerife/jerife.github.io
gh-badge: [star, follow]
tags: [Natural Language Processing]
comments: true
excerpt_separator: <!--break-->
---
<div align=center><h1>Attention Mechanism 이해하기</h1></div>
<!--break-->

----

 <br/>
## Attention란?
우리는 이전에 언어처리에 관한 Seq2Seq모델을 ["Introduce to Seq2Seq"](https://jerife.github.io/2021-06-08-seq2seq/)이글에서 다뤘습니다.<br/> 
Seq2Seq는 RNN모델(RNN, LSTM, GRU)등을 순환성을 기반으로 시퀀스 데이터(언어, 시계열 데이터 등)를 또 다른 시퀀스 데이터로 변환해주는 모델이라 했었습니다.

![image](https://user-images.githubusercontent.com/68190553/121048577-e2bcc580-c7f1-11eb-9c39-2cb0f717ce2d.gif){: .mx-auto.d-block :} 
>  Artchitecture of Seq2Seq

 <br/>
### 1) Seq2Seq의 한계
Seq2Seq의 한계는 Encoder -> Decoder로 보내는 Context Vector의 크기가 고정되었다는 것입니다.
<img width="27" alt="seq2seq_img_4" src="https://user-images.githubusercontent.com/68190553/121125066-e2a7de80-c860-11eb-8fd7-20e3e2ebefb5.png">{: .mx-auto.d-block :}

Context Vector는 입력값(“je suis étudiant”)의 모든 정보를 모아 놓은 동시에, 고정된 크기의 벡터이기도 합니다.<br/> <br/>
Context Vector는 많은양의 데이터도 꾹꾹 눌러서 압축시켜야하기 때문에, 정보 손실이 있다는 단점이 있었죠? 뿐만아니라 RNN계열의 특성상 Gradient Vanishing의 단점도 해결해야할 숙제였습니다.<br/> <br/>
이 한계를 극복하게해준 Mechanism이 ["Neural Machine Translation by Jointly Learning to Align and Translate
Dzmitry Bahdanau, Kyunghyun Cho, Yoshua Bengio"](https://arxiv.org/abs/1409.0473) 논문에서 제안된 바로 **"Attention"** 입니다.<br/> 


### 2) Attention Mechanism 
Attention Mechanism은 Decoder에서 출력 단어를 예측하는 매 시점마다, 인코더에서의 전체 입력 문장을 다시 한 번 참고한다는 점입니다.<br/> <br/>
Attention은 "집중"라는 의미를 지니고있습니다. Attention Mechanism에서도 입력 문장을 다시 한 번 참고할때, 해당 시점에서 예측해야할 단어와 연관이 있는 입력 단어 부분을 좀 더 집중(Attention)해서 보게 됩니다.

![attention_gif_1](https://user-images.githubusercontent.com/68190553/141924378-e7348509-de3e-4d72-944c-b62954a304bd.gif){: .mx-auto.d-block :}
위 사진은 Seq2Seq모델에 Attention을 적용했을때의 동작과정입니다. 지금부터 자세한 동작과정을 기술해보겠습니다.

---

### Seq2Seq with Attention 
#### [1] Prepare inputs
<img width="1147" alt="attention_img_2" src="https://user-images.githubusercontent.com/68190553/141924473-d9c88274-635d-4e91-9fc7-eee9a899b35b.png">{: .mx-auto.d-block :}

1-1) Seq2Seq의 Encoder 부분에서 각 단어마다 Hidden state가 나오며, 최종 출력단에 Context Vector가 나옵니다.<br/> 
1-2) 첫번째 Decoder는 "<START>"라는 입력과, 최종 출력단에 Context Vector를  Hidden state로 입력 받으며, 자신도 Output(Hidden state)을 출력합니다.

> 여기까진 Seq2Seq의 Mechanism 입니다.

 <br/>
#### [2] Score each hidden state
<img width="958" alt="attention_img_3" src="https://user-images.githubusercontent.com/68190553/141924521-cfe9da87-1916-4b63-88e5-d7b1b06e8c43.png">{: .mx-auto.d-block :}

2) 첫번째 Decoder의 Output(Hidden state)과 Encoder 부분에서 각 단어마다 Hidden state들을 dot product합니다.(이 값들을 **"Attention score"**라 합니다.)

 <br/>
#### [3] Softmax the scores
<img width="842" alt="attention_img_4" src="https://user-images.githubusercontent.com/68190553/141925277-a6d6160b-4d2d-4936-ac51-10448dfd7460.png">{: .mx-auto.d-block :}

3) 그리고 Attention score를 Softmax 취해줌으로써, Encoder 부분에서 각 단어마다 Hidden state들이 0~1사이 값을 가지게 됩니다.
   (첫번째 Decoder의 Output(Hidden state)과 연관 있을시 높은 점수를 얻습니다.)

 <br/>
#### [4] Sum up the weighted vectors
<img width="926" alt="attention_img_5" src="https://user-images.githubusercontent.com/68190553/141924653-6a52712d-4e39-4724-a6c0-02bafae5dab0.png">{: .mx-auto.d-block :}

4) 그리고 Softmax를 거친 Attention score과 Encoder 부분에서 각 단어마다 Hidden state를 곱한후, 더합니다.(weight sum)

 <br/>
#### [5] Concatnation with Decoders Hidden state and weighted vectors
5-1) weight sum한 값과 첫번째 Decoder의 Output(Hidden state)을 Concatnation해줍니다.<br/>
5-2) Concatnation한 값을 Layer넘겨 첫번째 Decoder 최종 Output 예측하고, 두번째 Decoder의 입력으로 값을 넘깁니다.

{: .box-warning} 
결국 Attention 은 위에서 언급한 것과 같이 Decoder에서 출력 단어를 예측하는 시점에서 예측해야할 단어와 연관이 있는 입력 단어 부분을 좀 더 집중하는 Mechanism이라는 것을 알수있죠.

---

### Performance
![attention_img_6](https://user-images.githubusercontent.com/68190553/141936561-810fb691-8665-4a37-aa94-5c1e92866443.png){: .mx-auto.d-block :}
어텐션 메커니즘의 유용한 속성은 해석이 가능하다는 점입니다. 입력 시퀀스의 특정 인코더 출력에 가중치를 부여하는 데 사용되므로 각 시간 단계에서 네트워크가 가장 집중되는 위치를 파악할 수 있습니다. <br/> <br/>
이상으로 Attention Mechanism에 대한 설명을 마치겠습니다.<br/> <br/>


###### Reference
*  https://arxiv.org/abs/1409.0473
*  https://jalammar.github.io/illustrated-transformer/
