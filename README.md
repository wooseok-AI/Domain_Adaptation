# Domain_Adaptation
<br>


## 1. Domain Adaptation Test on Mulitilingual-BERT using KLUE-NLI Dataset


<br>

[KLUE Benchmark](https://klue-benchmark.com/tasks/68/overview/description)에서 Natural Language Inference를 수행한다. [데이터 섹션](https://klue-benchmark.com/tasks/68/data/description) 에 설명돼 있듯이, 총 6개의 도메인이 포함되어 있다.

> **Premise문장을 참고하여 hypothesis 문장의 참, 거짓, 중립을 판별해야한다.**

```json
{
    "premise" : "씨름은 상고시대로부터 전해져 내려오는 남자들의 대표적인 놀이로서, 소년이나 장정들이 넓고 평평한 백사장이나 마당에서 모여 서로 힘과 슬기를 겨루는 것이다.",
    "hypothesis" : "씨름의 여자들의 놀이이다.",
    "label": "contradiction"
```
<br>

[Domain Adaptation](https://en.wikipedia.org/wiki/Domain_adaptation)
해당 프로젝트는 Multilingual BERT-base 모델이 Domain-Adaptation에 얼마나 효율적인지 보는 것이 주 목적이다.

1.  `Airbnb` 를 target domain으로 하여, 다른 domain train에 학습 시키고 target domain의 validation에서 성능을 측정
2. 두번째로는 target domain train에만 학습하고 validation에서 성능을 측정해서 두 수치를 비교

<br>

---

<br>


## 실험 구조
<br>

UDALM (KarouZos et al. https://doi.org/10.48550/arXiv.2104.07078) 의 아이디어를 일부 참고하여 Multilingual BERT를 MLM을 이용하여 Airbnb data로 Domain Pretraining을 수행하고, 이후 Fine-Tuning에서 Natural Language Inference 를 수행


실험은 총 **3가지 방향**으로 비교하며 진행하였다.
```
Test 1. Airbnb를 Target으로 나머지 Domain을 Source로 하여 Multilingual Bert를 Domain Pretrain을 수행하고, Source를 이용하여 Fine Tuning한 후 Target Validation을 이용하여 Accuracy 측정

Test 2. Target을 이용하여 Domain Pretrain과 Fine Tuning을 수행 한 후 Target Validation을 이용하여 Accuracy 측정

Test 3. Naive한 Multilingual BERT를 Target train으로만 Finetuning후, Validation에 대하여 Accuracy 측정.
```

<br>

---

## 결과

<br>

### <span style="color : #ffd33d">5 epoch<span>


|| Test 1 | Test 2 | Test 3 ||
|---|---|---|---|---|
|eval_loss|0.8943|0.9398|1.0084|
|eval_accuarcy|0.6033|0.5983|0.5583|

<br>

Test 3 과 비교 했을때, 미세한 차이로 Domain Adaptation이 효과가 있어 보이나, 큰 차이로 보이지 않아, Epoch을 10으로 늘려 시험하였다.

<br>

### <span style="color : #ffd33d">10 epoch<span>


|| Test 1 | Test 2 | Test 3 ||
|---|---|---|---|---|
|eval_loss|0.8465|0.9618|1.0064|
|eval_accuarcy|0.635|0.585|0.5567|


> Epoch 을 늘린 이후, (1) 방법론이 다른 방법론 보다 비교적 높은 성능을 보였다. 

<br>

---

## 결론
<br>

Multilingual BERT 모델에서의 Domin Adaptation이 효과적인 것을 알 수 있었다. 심지어 Target 도메인에 대해서 학습을 전혀 하지 못했음에도 불구하고, NLI라는 형식의 문장에 대해서 Perplexity 또한 16.588 에서 6.892로 감소했다. Target 만을 이용하여 ML-BERT를 Pretrain 하고 Finetuning한 모델은 낮은 성능을 보였는데, Target 데이터 자체가 Multilingual BERT의 성능을 높여줄 정도로 데이터량이 충분치 않았다는 가설을 세웠다. 이후 타 프로젝트에서 테스트해 볼 예정이다.




