---
title: '[Network] L2 오류 제어 vs L4 오류 제어 (L2 Error Handling vs L4 Error Handling)'
author: da-nyee
date: 2021-06-04 02:37:41 +0900
categories: [TIL, Network]
tags: [network, osi 7 layer, l2, data link layer, l4, transport layer, error handling]
---

## L2 Error Handling

- 오류 탐지(Error Detection) 역할 수행
- 오류가 있는 데이터 👉 상위단(ex. L3)로 전달할 필요 X 👉 바로 폐기처분하는 게 좋다.
- 프레임에 오류가 있는지 확인하는 역할은 수행하지만, 오류를 복구하는 역할은 수행하지 X 👉 이는 L4의 역할이다.
- 만약 L2에서 오류 제어를 하지 않으면, 굳이 보내지지 않아도 되는 데이터가 상위단으로 보내진다. 👉 패킷은 전송하는 비용이 크다. 👉 비효율적!
- 따라서, L2에서 오류 제어를 수행한다. 👉 데이터를 받는 쪽(수신측, destination)

## L4 Error Handling

- 오류 제어(Error Control) + 오류 복구(Error Recovery) 역할 수행
- source ↔️ destination 간 데이터가 문제 없이 전송됐는지 확인한다. 👉 중간에 데이터가 유실될 수도 있고, 여러 개의 전송 경로가 있는 경우에는 나중에 보낸 데이터가 먼저 도착할 수도 있기 때문이다.
- 패킷에는 Sequence Number 필드가 있다.
- 예를 들어, 데이터를 보내는 쪽(송신측, source)에서 1 / 2 / 3 / 4 번호를 부여해서 전송한다.
- 수신측에서 1 / 3 / 4 패킷만 받은 경우 👉 2번 패킷을 못 받았으므로 송신측에 해당 데이터를 다시 보내달라 요청한다. 👉 송신측에서 2번 패킷을 다시 보내준다.
- 따라서, 정확한 데이터 전송을 보장한다. 👉 TCP!
- 중요한 건! 상호간에 이상 없이 데이터가 전송됐는지 확인하고, 만약 이상이 있으면 데이터를 재전송하는 일련의 작업을 수행한다.

## References

- [OSI 7 Layer : 'Layer 2'의 역할](https://4network.tistory.com/entry/basic20100303)
- [OSI 7 Layer : 'Layer 4'의 역할](https://4network.tistory.com/entry/basic20100328)