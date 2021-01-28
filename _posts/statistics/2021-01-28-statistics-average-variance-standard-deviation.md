---
title: '[Statistics] 평균, 분산, 표준편차 (Average, Variance, Standard Deviation)'
author: da-nyee
date: 2021-01-28 22:03:28 +0900
categories: [TIL, Statistics]
tags: [statistics, average, variance, standard deviation]
---

## Average

- 평균은 대표값 중 하나이다.
- 평균은 성과 측정 기준이 될 수 없다.
- 평균은 <b>데이터의 들쭉날쭉한 정도를 반영하지 않기 때문</b>이다.

```
average = sum(data) / len(data)
```

## Deviation

```
deviation[i] = data[i] - average
```

## Squared Deviation

```
squared_deviation[i] = deviation[i] ** 2
```

## Variance

- 분산은 데이터의 퍼져 있는 정도를 나타낸다.
- 분산은 <b>데이터의 들쭉날쭉한 정도를 알아내기 위해 계산</b>한다.
- 분산이 크다 == 데이터가 넓게 분포한다. 데이터가 들쭉날쭉하다.
- 분산이 작다 == 데이터가 좁게 분포한다. 데이터가 들쭉날쭉하지 않다.

![variance](https://user-images.githubusercontent.com/50176238/106118006-f8144a00-6196-11eb-9656-ee280117a90f.PNG)

```
variance = sum(squared_deviation) / len(squared_deviation)
```

## Standard Deviation

- 표준편차는 분산에 제곱근(루트)를 취해 줄인 값이다.
- 표준편차는 <b>평균적인 들쭉날쭉함으로 지표화</b>된다.

```
from math import sqrt

standard_deviation = sqrt(variance)
```

## Conclusion

- 데이터(변량) ➡ 평균 ➡ 편차 ➡ 편차제곱 ➡ 분산 ➡ 표준편차

## References

- [통계의 기초인 평균, 분산, 표준편차](https://learnx.tistory.com/entry/%ED%86%B5%EA%B3%84%EC%9D%98-%EA%B8%B0%EC%B4%88%EC%9D%B8-%ED%8F%89%EA%B7%A0-%EB%B6%84%EC%82%B0-%ED%91%9C%EC%A4%80%ED%8E%B8%EC%B0%A8)
- [기본적인 통계지식 익히기 — 평균, 분산, 표준편차](https://nion9.medium.com/%EA%B8%B0%EB%B3%B8%EC%A0%81%EC%9D%B8-%ED%86%B5%EA%B3%84%EC%A7%80%EC%8B%9D-%EC%9D%B5%ED%9E%88%EA%B8%B0-%ED%8F%89%EA%B7%A0-%EB%B6%84%EC%82%B0-%ED%91%9C%EC%A4%80%ED%8E%B8%EC%B0%A8-9f840d10ec8)
- [[수리통계학] 6. 분산 및 표준편차](https://analysisbugs.tistory.com/7)