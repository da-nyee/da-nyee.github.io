---
title: '[Kafka] Kafka 튜토리얼 (Kafka Tutorial)'
author: da-nyee
date: 2022-01-06 02:06:02 +0900
categories: [TIL, Kafka]
tags: [kafka, tutorial]
---

## Kafka?

- 비동기 처리를 위한 메시징 큐의 한 종류
- ZooKeeper, Broker, Producer, Consumer와 같은 개념이 있다.

### 주키퍼(ZooKeeper)

- 브로커의 메타 정보를 관리하고, 브로커의 컨트롤러(Controller)를 선출하는 역할을 한다.
- 주키퍼 앙상블(ZooKeeper Ensemble)
    - 주키퍼의 신뢰도 높은 서비스를 위해 클러스터(Cluster)라는 노드 세트를 구성한 것이다.
    - 주키퍼는 과반수 투표 방식을 따른다.
    - 그래서 살아 있는 노드가 과반이면, 지속적인 서비스를 제공할 수 있다.
    - e.g. 주키퍼 앙상블을 3대로 구성한다면? 서버 1대에 장애가 발생해도 문제가 없다. 남은 2대로 안정적인 서비스를 제공할 수 있다.
    - 그러므로 주키퍼 앙상블은 홀수로 설정하는 것이 좋다.

### 브로커(Broker)

- 일반적으로 <b>카프카</b>라고 불리는 시스템을 말한다.
- 즉, 카프카 자체이다.

<img style="width: 520px; height: 300px" src="https://user-images.githubusercontent.com/50176238/148253036-02c208cd-bc1f-45e7-98ea-34588fa0f859.png">

> 이미지 출처: [[Kafka 101] 카프카 브로커 (Kafka Broker)](https://always-kimkim.tistory.com/entry/kafka101-broker)

### 프로듀서(Producer)

- <b>이벤트를 발행</b>하는 주체
- 카프카와는 별도인 애플리케이션이다.

### 컨슈머(Consumer)

- <b>이벤트를 구독</b>하는 주체
- 카프카와는 별도인 애플리케이션이다.

<img style="width: 456px; height: 340px" src="https://user-images.githubusercontent.com/50176238/148254338-05d5a917-29b1-42c0-b420-cb9a9ee74448.png">

> 이미지 출처: [Kafka 운영자가 말하는 처음 접하는 Kafka](https://www.popit.kr/kafka-%ec%9a%b4%ec%98%81%ec%9e%90%ea%b0%80-%eb%a7%90%ed%95%98%eb%8a%94-%ec%b2%98%ec%9d%8c-%ec%a0%91%ed%95%98%eb%8a%94-kafka/)

<br/>

## Quick Start

### 카프카 설치

- [다운로드](https://www.apache.org/dyn/closer.cgi?path=/kafka/3.0.0/kafka_2.13-3.0.0.tgz)
- 압축 해제

```bash
$ tar -xzf kafka_2.13-3.0.0.tgz
$ cd kafka_2.13-3.0.0.tgz
```

### 카프카 시작

- 주키퍼 실행
    - 터미널 1개 실행

```bash
$ bin/zookeeper-server-start.sh config/zookeeper.properties
```

- 브로커 실행
    - 터미널 1개 실행

```bash
$ bin/kafka-server-start.sh config/server.properties
```

### 토픽 생성

- 토픽(Topic)은 데이터를 구분하기 위한 일종의 저장소이다.
- e.g. 아파트의 우편함, 이메일의 메일함
- 같은 토픽은 같은 문맥(Context)를 가진다.
- e.g. 주문(Order)이라는 토픽이 있다면, 주문에 대한 이벤트(주문 생성, 주문 취소 등)가 발행/구독된다.

```bash
# 다양한 옵션을 지정할 수 있다.
$ bin/kafka-topics.sh --create --partitions 1 --replication-factor 1 --topic quickstart-events --bootstrap-server localhost:9092
```

### 이벤트 발행

- Producer Client
    - 터미널 1개 실행

```bash
$ bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
```

### 이벤트 구독

- Consumer Client
    - 터미널 1개 실행

```bash
$ bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
```

### 최종 모습

- 터미널 4개를 실행했고, 각 역할에 따라 구분했다.

<img src="https://user-images.githubusercontent.com/50176238/148248008-2999c725-86b0-4cba-98d0-1e17e0262b71.png">

- 프로듀서에서 이벤트를 발행하면 컨슈머에서 이벤트를 구독하는 형태이다.

<img src="https://user-images.githubusercontent.com/50176238/148248124-525e4478-d537-4d7f-b2be-4fd7d34b73a6.png">

<br/>

## Referenecs

- [Kafka 운영자가 말하는 처음 접하는 Kafka](https://www.popit.kr/kafka-%ec%9a%b4%ec%98%81%ec%9e%90%ea%b0%80-%eb%a7%90%ed%95%98%eb%8a%94-%ec%b2%98%ec%9d%8c-%ec%a0%91%ed%95%98%eb%8a%94-kafka/)
- [[Kafka 101] 카프카 브로커 (Kafka Broker)](https://always-kimkim.tistory.com/entry/kafka101-broker)
- [[kafka] 카프카 주키퍼](https://log-laboratory.tistory.com/149)
- [APACHE KAFKA QUICKSTART](https://kafka.apache.org/quickstart)
- [[카프카] 토픽(Topic)과 파티션(Partition) 이해](https://needjarvis.tistory.com/603)
- [[Kafka 101] 카프카 메시지와 토픽과 파티션 (Kafka Message, Topic and Partition)](https://always-kimkim.tistory.com/entry/kafka101-message-topic-partition)