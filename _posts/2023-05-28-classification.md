---
title: True False Positive Negative Matrix
tags: classification ml tp tn fp fn
---

Apple Create ML을 공부하던 차에, Classification 관련 용어를 정리해봤다.

## Definition

### True vs False
  - 예측이 진짜(True)인지 가짜(False)인지를 의미

### Positive vs Negative
  - 예측 값 자체 `양성(Positive) 또는 음성(Negative)`를 의미

이를 조합하면 다음의 4가지가 나온다.

## 늑대 예측하기
- True Positive (TP)
  - 양성이라고 예측했는데, 그 예측이 진짜였다. (늑대라고 예측했는데, 진짜 늑대였다. 👍)
- False Positive (FP)
  - 양성이라고 예측했는데, 그 예측이 가짜였다. (늑대라고 예측했는데, 늑대가 아니었다. 👎)
- True Negative (TN)
  - 음성이라고 예측했는데, 그 예측이 진짜였다. (늑대가 아니라고 예측했는데, 진짜 늑대가 아니었다. 👍)
- False Negative (FN)
  - 음성이라고 예측했는데, 그 예측이 가짜였다. (늑대가 아니라고 예측했는데, 늑대였다. 👎)

---
Reference [Google Docs](https://developers.google.com/machine-learning/crash-course/classification/true-false-positive-negative)