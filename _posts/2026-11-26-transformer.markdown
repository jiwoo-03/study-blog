---
layout: post
title: "Transformer Model"
date: 2026-11-26
---
# Transformer Arvhitecture
Transformer는 Attention만으로 만든 번역 모델로써, 2017년 구글 연구팀의 논문 **"Attention is All You Need"**에서 처음 제안됐다.

![Transformer Architecture]({{ site.baseurl }}/assets/images/Transfomer%20Architecture%20img.png)

## Encoder & Decoder

### Encoder
**인코더란 Input으로 들어온 데이터를 컴퓨터가 이해할 수 있는 Tensor 형태로 변환하는 곳**으로써 쉽게 말하면 번역 과정에서 컴퓨터가 원문을 읽고 이해하는 단계이다.

Transformer의 Encoder에는 담은 과 같은 시스템이 작용한다.
- `Input Embedding` 
- `Positional Encoding` 
- `Self Attention(Muti-Head Attention)`
- `Add & Norm`
- `Feed Forward`

### Decoder
**디코더는 인코더에서 컴퓨터가 이해할수 있는 언어로 변환된 데이터들을 다시 사람이 이해할 수 있는 데이터로 변환 시켜 출력하는 곳**을 만한다. 번역의 과정에서는 번역문을 쓰는 단계라고 생각하면 쉽다.

Decoder에서는 다음과 같은 시스템이 작용한다.
- `Output Embedding`
- `Positional Encoding`
- `Masked Attention(Masked Muti-Head Attention)`
- `Cross Attention(Multi-Head Attention)`
- `Add & Norm`
- `Feed Forward`
- `Linear and Softmax`

## Attention 
일단 먼저 Transformer를 알기전 가장 먼저 할 일은 Transformer에서 가장 중요한 단계인 `Attention`에 대해서 알아야한다.

`Attention`이란 **각 단어의 정보를 관련도 비율만큼 섞어서 지금 필요한 문맥정보를 가져오는 방법**을 말한다.

`Attention`의 `Query(Q)`,`Key(K)`,`Value(V)`3단계로 나누어져 있다.
"I Love you"원문을 "나는 널 사랑해"라는 문장으로 번역한다고 해보자.
그러면 "I Love you"라는 문장은 각 토큰 별로 쪼개지고 Embedding을 거쳐 Tensor 형태로 변환된다.

이후 Tensor들은 Weighted_Q, Weighted_K, Weighted_V와 행렬곱형태로 결합되어 각각 Query, Key, Value 형태의 Tensor로 변환되며, Query와 K들의 Tensor들과 서로 관련도 점수를 매긴다. 

그렇게 하면 각각의 Tensor들은 관련도 점수가 -/+가 섞인 점수들이 나타나게 되고 그걸 Softmax를 통해서 각 Tensor들의 합이 1이 되는 확률 형태로 변환시킨다.

마지막으로 Value와의 가중합을 통해 하나의 Tensor로 출력되는 것이 `Attention` 과정이다.

## Self Attention 
그러나 Transformer의 경우 `Attention` 방식이 아닌 `Self Attention` 방식을 쓴다. 기존 `Attention`의 경우 "I Love you"와 "나는 널 사랑해"라는 문장을 Tensor 형태로 변환 후 "I Love you"의 Tensor는 Q가 되며, "나는 널 사랑해"라는 문장에 대한 Tensor는 Key,Value로 변환된다 그러나 `Self Attention`의 경우 한 문장 즉, **하나의 시퀀스가 Q,K,V 모두 들어가 사용된다는 점이 기존 방식과 차이가 있다.**

그렇게 하면 "I Love you"는 Encoder에 들어가게 되고 "나는 널 사랑해"라는 문장은 Decoder에 들어가서 나중에 "나는 널 사랑해"라는 문장을 번역하는 과정에서 "I Love you"를 참고하여 번역을 쉽게 할 수 있게 되는 것이다.

## Positional Encoding
Input 과 Output에 들어가는 문장 sequence에 **위치 정보를 주는 역할을 합니다.** 하나의 예시로 "나는 고기를 좋아합니다."과 "고기는 나를 좋아합니다."라는 문장은 한눈에 봐도 차이가 보입니다. 고기는 나를 좋아할 수 없겠죠 그렇기 때문에 이러한 문장 속 단어들에 `Postional Encoding`을 부여하여 문장 속 단어들의 위치를 파악할 수 있도록 심어주는 것 입니다.

`Positional Encoding`은 sin·cos을 이용하여 위치마다 sin·cos을 계산하여 한 쌍으로 묶습니다. sin·cos 함수를 사용함으로써 위치가 변할 때마다 원 위의 좌표처럼 움직이며 -1~1 사이 값을 반환합니다. 또한 서로다른 속도로 변하는 즉 파동이 다른 sin·cos 쌍을 이용하여 중복되는 값이 없도록 위치정보를 나타냅니다. 

## Masked Self Attention
`Masked Self Attention`의 경우 `Self Attention`과 다르게 학습 할 다음 문장을 **Masking**하여 보이지 않게 만든 다음 예측 하도록 만든 것입니다. 결국 번역을 한다고 가정할 때 번역할 문장은 Decoder에 들어가며 이 문장은 Model이 예측해야하는 문장입니다.

토큰별로 문장을 나눴을 때 현재 문장은 다음 단어 볼 수 없어야 하며, 자기자신과 그전 단어만을 보고 다음 단어를 예측해야 하기 때문에 **Masking**을 통해 학습하는 것입니다.

"나는 널 사랑해"라는 문장은 `Self Attention`에 의해 행렬곱 형태의 병렬 계산으로 한 단어가 다른 단어들과의 유사도를 검증 합니다.
여기서 `Maksed Attention`은 "나는 널 사랑해"라는 문장 합에 `<Start of Sequence>`라는 Tensor를 붙여 줍니다. 이걸 붙여주는 이유는 예측할 때, 먼저 시작점이 필요하기 때문이라고 할 수 있으며, 토큰을 붙였을 때 "<sos> 나는 널 사랑해" 라는 문장의 형태로 들어가 학습을 하게 되는 것입니다.

그러면 "나는"이라는 토큰 하나는 결국 `Self Attention`에 의해 자기 자신과 `<sos>`만을 학습 하게 되고 이를 통해 데이터 누수 현상을 방지 할 수 있게 되는 것입니다.

## Cross Attention
`Masked Attention`을 통해 학습된 "<sos>나는 널 사랑해"라는 문장은 `Cross Attention`으로 가서 Encoder에서 학습 했던 "I Love you"라는 문장 sequence를 참고하여 다음 문장을 예측합니다.

이 과정을모두 거치게 되면 출력 형태의 문장 뒤에는 `<End of Sequence>`가 붙어 나오게 됩니다. 그 이유는 앞에서 `<sos>`를 붙였기 때문에 마지막 문장을 예측하고 마무리 하기 위해 붙여 준다 라고 생각하면 됩니다.

## Feed Forward 
`FFN`은 `Linear -> ReLU -> Linear` 과정을 거칩니다. 원논문 에서는 512차원의 `Network`를 2048개로 확장 시킨 후 `ReLU` 비선형 변환을 거쳐 다시 512차원으로 축소하는 과정을 거칩니다.

그 이유는 `FFN`을 통해 Transformer에 들어온 Tensor 형태의 데이터를 확장 시켜 각각 특징을 뽑기 위해서 입니다. 또한 `ReLU` 변환을 거칠때 -값들은 ReLU 함수에 의해 0으로 변환되며 그로인한 손실을 막기 위한 이유도 있다고 볼 수 있습니다. 

정리하면 `FFN` 확장 과정에서 다양한 특징을 뽑아내고 축소 과정을 통해 다시 Decoder에 연결 할 수 있는 형태로 변환하는 과정인 것입니다 그과정 에서 ReLU 함수를 거쳐 0값으로 손실되는 것을 방지하는 목적또한 포함 하고 있다고 볼 수 있습니다.

## Add & Norm 

### Add(Resdiual)
`Residual`은 잔차 결합이라고하며, `FFN`과 `Attention`과정을 통해 손실 된 정보를 보존 하고자 기존 에 들어갔던 Data를 결합하여 데이터 손실을 방지하는 역할 을 합니다.

### Normalization
`Residual`로 계속 더하다 보면 값이 폭주하는 현상이 발현되고 그걸 막기 위해 정규화 작업을 거칩니다. 

