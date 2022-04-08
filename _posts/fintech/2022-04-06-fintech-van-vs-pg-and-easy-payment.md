---
title: '[Fintech] VAN vs PG, 그리고 간편 결제'
author: da-nyee
date: 2022-04-06 03:20:02 +0900
categories: [TIL, Fintech]
tags: [fintech, van, pg, easy payment]
---

## VAN

![van-offline](https://user-images.githubusercontent.com/50176238/161807503-f37ac703-4deb-4f11-9fc3-24109675d7dd.png)

> 이미지 출처: [PG와 VAN 무엇이 다를까?](https://blog.tosspayments.com/articles/semo-16)

<br/>

- Value Added Network
- 오프라인 결제 (카드 단말기, POS 단말기)
- 가맹점은 VAN사 덕분에 카드사와 편리하게 결제를 연동할 수 있다.<br/>
하지만 VAN사가 매출 정산은 따로 해주지 않아, 각 카드사마다 매출을 정산받아야 한다.
- 만약 VAN사가 없었다면? 가맹점은 각 카드사와 결제 계약을 맺고 사용해야 한다.<br/>
가맹점마다 쓸 수 있는 카드가 달랐을 것이다.
- e.g. NHN한국사이버결제, 나이스정보통신, 한국정보통신

<br/>

## PG

![pg-online](https://user-images.githubusercontent.com/50176238/161807928-e96f2f7c-72ca-4183-9bb8-22399c4a0e64.png)

> 이미지 출처: [PG와 VAN 무엇이 다를까?](https://blog.tosspayments.com/articles/semo-16)

<br/>

- Payment Gateway
- 온라인 결제
- 가맹점은 PG사 덕분에 카드사와 편리하게 결제를 연동하고, 매출을 정산받을 수 있다.<br/>
즉 PG사를 통해 여러 카드사와 한번에 거래하고, PG사와 계약한 정산일에 전체 매출액을 한번에 정산받는다.
- e.g. NHN한국사이버결제, KG이니시스

<br/>

## 간편 결제

- 앱에 카드를 등록해두면
    - 온라인에서는 6자리 비밀번호만 입력해서
    - 오프라인에서는 QR 코드와 같은 것을 사용해서 결제할 수 있게 하는 서비스
- 간편 결제는 양면시장이다. 가맹점과 유저를 동시에 모아야 한다. 그래서 마케팅을 많이 한다.
- 기업은 왜 간편 결제에 집중할까? 양면시장은 어느 한쪽에 경쟁력이 클수록 다른 한쪽에 협상력을 강하게 갖는다.
    - 유저가 많다면, 가맹점이 먼저 찾을 수 있다. (저 간편 결제를 쓰면 매출에 도움이 되겠구나)
    - 가맹점이 많다면, 유저는 편리해서 더 많이 이용할 수 있다.
- 요즘은 오프라인 가맹점을 유치하기 위해 힘쓰는데, 이는 O2O(Online to Offline) 때문이다.<br/>
O2O는 온라인에서 결제하고 오프라인에서 재화/서비스를 받는 것을 의미한다.
    - 유저는 계산대에 줄을 서서 기다리지 않아도 된다.
    - 유저는 카드/현금을 소지하지 않아도 된다. 모바일로 편리하게 결제할 수 있다.
- e.g. 카카오페이, 네이버페이, 토스페이

<br/>

## References

- [[New머니] 간편결제 뒤에는 'PG'와 'VAN'이 있다... 시장 변화 속 더 뜨는 'PG'](https://www.techm.kr/news/articleView.html?idxno=70586)
- [PG와 VAN 무엇이 다를까?](https://blog.tosspayments.com/articles/semo-16)
- [결제산업 완전분석: ⑦온라인 결제와 간편 결제의 등장](https://yozm.wishket.com/magazine/detail/1209/)