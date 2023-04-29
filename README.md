# Domain_Adaptation
Domain Adaptation Test on Mulitilingual-BERT using KLUE-NLI Dataset

## 1. Natural Language Inference with Domain Adaptation.

KLUE Benchmark에서 Natural Language Inference를 수행한다.
(https://klue-benchmark.com/tasks/68/overview/description). 
데이터 섹션 (https://klue-benchmark.com/tasks/68/data/description) 에 설명돼 있듯이, 총 6개의 도메인이 포함되어 있다.

- **Premise문장을 참고하여 hypothesis 문장의 참, 거짓, 중립을 판별해야한다.**

```
premise: 씨름은 상고시대로부터 전해져 내려오는 남자들의 대표적인 놀이로서, 소년이나 장정들이 넓고 평평한 백사장이나 마당에서 모여 서로 힘과 슬기를 겨루는 것이다.
hypothesis: 씨름의 여자들의 놀이이다.

label: contradiction
```

해당 프로젝트는 Multilingual BERT-base 모델이 Domain Adaptation (https://en.wikipedia.org/wiki/Domain_adaptation) 에 얼마나 효율적인지 보는 것이 주 목적이다.

(1) `Airbnb` 를 target domain으로 하여, 다른 domain train에 학습 시키고 target domain의 validation에서 성능을 측정
(2) 두번째로는 target domain train에만 학습하고 validation에서 성능을 측정해서 두 수치를 비교

---

##실험 단계

UDALM (KarouZos et al. https://doi.org/10.48550/arXiv.2104.07078) 의 아이디어를 일부 참고하여 Multilingual BERT를 MLM을 이용하여 Airbnb data로 Domain Pretraining을 수행하고, 이후 Fine-Tuning에서 Natural Language Inference 를 수행

실험은 총 3가지 방향으로 진행하였다.

(1) Airbnb를 Target으로 나머지 Domain을 Source로 하여 Multilingual Bert를 Domain Pretrain을 수행하고, Source를 이용하여 Fine Tuning한 후 Target Validation을 이용하여 Accuracy 측정

(2) Target을 이용하여 Domain Pretrain과 Fine Tuning을 수행 한 후 Target Validation을 이용하여 Accuracy 측정

(3) Naive한 Multilingual BERT를 Target train으로만 Finetuning후, Validation에 대하여 Accuracy 측정.

결과는 다음과 같다

### 5 epoch

(1) Accuracy: 0.603

'eval_loss': 0.8943072557449341
'eval_accuracy': 0.6033333333333334
(2) Accuracy: 0.598

'eval_loss': 0.9398565888404846
'eval_accuracy': 0.5983333333333334
(3) Accuracy: 0.558

'eval_loss': 1.0084370374679565
'eval_accuracy': 0.5583333333333333
방법론 (3)과 비교 했을때, 미세한 차이로 Domain Adaptation이 효과가 있어 보이나, 큰 차이로 보이지 않아, Epoch을 10으로 늘려 시험하였다.

### 10 epoch

Result

(1) ***Accuracy: 0.635***

  *   'eval_loss': 0.8465157151222229
  *  'eval_accuracy': 0.635

(2) ***Accuracy: 0.585***


*   'eval_loss': 0.9618410468101501
*  'eval_accuracy': 0.585

(3) ***Accuracy: 0.557***


*   'eval_loss': 1.0064477920532227 
* 'eval_accuracy': 0.5566666666666666,

Epoch 을 늘린 이후, (1) 방법론이 다른 방법론 보다 비교적 높은 성능을 보였다. 

