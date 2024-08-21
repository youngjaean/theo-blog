---
{"dg-publish":true,"permalink":"/1-paper/130-theory/few-shot-learning-survey/","title":" Few-Shot Learning Survey","tags":["summary","paper"]}
---

# Few-Shot Learning Survey

## 1. Few-Shot Learning이란?

Few-Shot Learning은 적은 수의 학습 데이터로 새로운 작업을 수행할 수 있는 기계학습 방법입니다.

### As-Is (기존 방식)
- 새로운 데이터를 학습하기 위해 많은 양의 데이터가 필요했습니다.

### To-Be (Few-Shot Learning)
- 적은 수의 예제로 광범위한 일반화가 가능합니다.

## 2. Few-Shot Learning 방법론

### 2.1 Metrics Based Methods

#### 2.1.1 Siamese Network
- 두 개의 다른 이미지를 입력으로 받습니다.
- 가중치(weights)와 편향(bias)을 공유합니다.
- Loss function:
  $Loss = (1-Y) \cdot \frac{1}{2} \cdot (D_w)^2 + Y \cdot \frac{1}{2} \cdot \max(0, m - D_w)^2$
  여기서 첫 번째 항은 Mean Squared Error, 두 번째 항은 Hinge Loss입니다.

#### 2.1.2 Matching Network
- 주요 공식:
  $a(x, x_i) = \frac{\exp(\cos(f(x), g(x_i)))}{\sum_j \exp(\cos(f(x), g(x_j)))}$
- 양방향 LSTM을 사용하여 support set을 인코딩합니다.
- 테스트 셋에 대해 read attention과 함께 LSTM을 이용하여 인코딩합니다.

### 2.2 Models Based Methods

#### 2.2.1 Memory Augmented Neural Networks (MANN)
- Reading 과정:
  $r_t \leftarrow \sum_i R(w_t^r)_i \cdot M_t(i)$
- Writing 과정:
  - Least Recently Used Access(LRUA) 사용
  - 유사한 데이터는 덮어쓰고, 새로운 데이터는 잘 사용되지 않은 메모리에 저장

#### 2.2.2 Meta Networks
- $F_w$: embedding function f의 fast weight를 학습하기 위한 LSTM
- $G_w$: base learner g의 loss gradient로부터 fast weight를 학습하기 위한 신경망
- learner의 loss gradient를 task에 대한 meta information으로 정의

### 2.3 Optimization based Methods

#### 2.3.1 Model Agnostic Meta Learning (MAML)
- 주요 업데이트 규칙:
  $\theta^* = \arg\min_\theta \sum_{T_i\sim p(\tau)} L^{\tau_i}_1(f_{\theta_i'})$
  $\theta_i' = \arg\min_\theta \sum_{T_i\sim p(\tau)} L^{\tau_i}_1(f_{\theta-\alpha\nabla_\theta L^{\tau_i}_0(f_\theta)})$
  $\theta \leftarrow \theta - \beta\nabla_\theta \sum_{T_i\sim p(\tau)} L^{\tau_i}_1(f_{\theta-\alpha\nabla_\theta L^{\tau_i}_0(f_\theta)})$

#### 2.3.2 LSTM-Meta Learning
- 역전파의 그래디언트 기반 업데이트와 유사합니다.
- 시간 t에서의 학습률을 사용하여 학습:
  $\theta_t = \theta_{t-1} - \alpha_t \nabla_{\theta_{t-1}} L_t = f_t \odot \tilde{c}_{t-1} + i_t \odot \tilde{c}_t$

## 3. 결론

- Models Based Methods는 아직 실전 적용에 어려움이 있습니다.
- 기존에 학습된 모델을 기반으로 새로운 데이터를 학습하는 방법이 효과적입니다.