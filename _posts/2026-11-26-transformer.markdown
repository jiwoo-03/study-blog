---
layout: post
title: "Transformer Model"
date: 2026-11-26
---
# Transformer Arvhitecture
Transformer는 Attention만으로 만든 번역 모델로써, 2017년 구글 연구팀의 논문 **Attention is All You Need**에서 처음 제안됐다.

## Encoder & Decoder 

### Encoder
인코더란 Input으로 들어온 데이터를 컴퓨터가 이해할 수 있는 Tensor 형태로 변환하는 곳으로써 쉽게 말하면 번역 과정에서 컴퓨터가 원문을 읽고 이해하는 단계이다.

Transformer의 Encoder에는 담은 과 같은 시스템이 작용한다.
- `Input Embedding` 
- `Positional Encoding` 
- `Self Attention(Muti-Head Attention)`
- `Add & Norm`
- `Feed Forward`

### Decoder
디코더는 인코더에서 컴퓨터가 이해할수 있는 언어로 변환된 데이터들을 다시 사람이 이해할 수 있는 데이터로 변환 시켜 출력하는 곳을 만한다. 번역의 과정에서는 번역문을 쓰는 단계라고 생각하면 쉽다.

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

`Attention`이란 각 단어의 정보를 관련도 비율만큼 섞어서 지금 필요한 문맥정보를 가져오는 방법을 말한다.

`Attention`의 `Query(Q)`,`Key(K)`,`Value(V)`3단계로 나누어져 있다.
"I Love you"원문을 "나는 널 사랑해"라는 문장으로 번역한다고 해보자.
그러면 "I Love you"라는 문장은 각 토큰 별로 쪼개지고 Embedding을 거쳐 Tensor 형태로 변환된다.

이후 이 Tensor들은 Weighted_Q, Weighted_K, Weighted_V와 행렬곱형태로 결합되어 각각 Query, Key, Value 형태의 Tensor로 변환되며, Query와 K들의 Tensor들과 서로 관련도 점수를 매긴다. 

그렇게 하면 각각의 Tensor들은 관련도 점수가 -/+가 섞인 점수들이 나타나게 되고 그걸 Softmax를 통해서 각 Tensor들의 합이 1이 되는 확률 형태로 변환시킨다.

이후 Value와의 가중합을 통해 하나의 Tensor로 출력되는 것이 `Attention` 과정이다.

## Self Attention 
그러나 Transformer의 경우 `Attention` 방식이 아닌 `Self Attention` 방식을 쓴다. 기존 `Attention`의 경우 "I Love you"와 "나는 널 사랑해"라는 문장을 Tensor 형태로 변환 후 "I Love you"의 Tensor는 Q가 되며, "나는 널 사랑해"라는 문장에 대한 Tensor는 Key,Value로 변환된다 그러나 `Self Attention`의 경우 한 문장 즉, 하나의 시퀀스가 Q,K,V 모두 들어가 사용된다는 점이 기존 방식과 차이가 있다.

그렇게 하면 "I Love you"는 Encoder에 들어가게 되고 "나는 널 사랑해"라는 문장은 Decoder에 들어가서 나중에 "나는 널 사랑해"라는 문장을 번역하는 과정에서 "I Love you"를 참고하여 번역을 쉽게 할 수 있게 되는 것이다.

## Positional Encoding

