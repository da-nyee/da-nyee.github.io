---
title: '[Cloud Computing] AWS, Azure, GCP 비교'
author: da-nyee
date: 2020-09-16 00:13:16 +0900
categories: [TIL, Cloud Computing]
tags: [cloud computing, aws, azure, gcp, comparison]
---

## Introduction

3대 클라우드 서비스인 AWS, Azure, GCP를 비교해보자!

## Overall Features

|                             |<center>AWS</center>|<center>Azure</center>|<center>GCP</center>|
|-----------------------------|--------------------|----------------------|--------------------|
|<center><b>로그인</b></center>|- 별도 AWS 계정 사용|- Microsoft 계정 사용<br/>- Github 계정 사용 가능|- Google 계정 사용|
|<center><b>무료 계정</b></center>|- AWS Free Tier<br/>- 12개월 무료<br/>- 크레딧 제공 없음|- 체험 계정<br/>- 12개월 무료<br/>- ₩224,930 크레딧 제공|- 무료 등급<br/>- 3개월 무료<br/>- $300 크레딧 제공|
|<center><b>교육 계정</b></center>|- AWS Educate<br/>- 교육기관 미등록 시 $75 크레딧 제공<br/>- 교육기관 등록 시 $100 크레딧 제공<br/>- 일부 제품 사용 제한|- Azure for Students<br/>- $100 크레딧 제공<br/>- 일부 제품 사용 제한|- Google for Education<br/>- $50 크레딧 제공<br/>- 일부 제품 사용 제한|
|<center><b>마켓<br/>플레이스</b></center>|- 약 70개 카테고리 존재<br/>- 분야별 카테고리 정리<br/>- 9083개 제품 존재|- 약 20개 카테고리 존재<br/>- 무작위 카테고리 정리|- 약 15개 카테고리 존재<br/>- 무작위 카테고리 정리<br/>- 3901개 제품 존재|

## Product Comparsion

|                             |<center>AWS</center>|<center>Azure</center>|<center>GCP</center>|
|-----------------------------|--------------------|----------------------|--------------------|
|<center><b>Computing</b></center>|\[서비스 개수\]<br/>- 11개<br/><br/>\[주요 서비스\]<br/>- EC2<br/>- Lambda<br/><br/>\[특징\]<br/>- 300개 이상 인스턴스 생성 가능<br/>- 100Gbps 이더넷 지원<br/>- Azure보다 7배 빠른 가동 중단 시간|\[서비스 개수\]<br/>- 20개<br/><br/>\[주요 서비스\]<br/>- 가상 머신<br/>- Azure Function<br/><br/>\[특징\]<br/>- 일관된 하이브리드 클라우드 기술 활용<br/>- 복잡성 추가 없이 인프라 확장 가능<br/>- 컴퓨팅, 메모리 최적화|\[서비스 개수\]<br/>- 12개<br/><br/>\[주요 서비스\]<br/>- Compute Engine<br/>- 단독 테넌트 노드<br/><br/>\[특징\]<br/>- 라이브 마이그레이션<br/>- OS 패치 관리<br/>- 간편한 통합 및 필요에 따른 전역 확장<br/>- 하드웨어 투명성|
|<center><b>Analysis</b></center>|\[서비스 개수\]<br/>- 12개<br/><br/>\[주요 서비스\]<br/>- Amazon QuickSight<br/><br/>\[특징\]<br/>- 간편한 데이터 레이크 및 분석 구축<br/>- 확장성 및 비용 효율성<br/>- 포괄적, 개방적 서비스<br/>- 안전한 분석 인프라|\[서비스 개수\]<br/>- 15개<br/><br/>\[주요 서비스\]<br/>- Azure Analysis Services<br/><br/>\[특징\]<br/>- 비즈니스 속도에 맞게 성능 조정<br/>- 효율적인 확장<br/>- 강력한 보안성<br/>- 친숙한 개발 환경|\[서비스 개수\]<br/>- 12개<br/><br/>\[주요 서비스\]<br/>- BigQuery<br/><br/>\[특징\]<br/>- 간편한 데이터 분석<br/>- 즉각적인 통계 도출<br/>- 예측적 통계를 통한 분석가 지원<br/>- 데이터 자동 암호화|
|<center><b>Database</b></center>|\[서비스 개수\]<br/>- 12개<br/><br/>\[주요 서비스\]<br/>- Amazon DynamoDB<br/>- Amazon RDS<br/><br/>\[특징\]<br/>- 대규모 성능<br/>- 완전관리형 DB<br/>- 업무용 사용 가능<br/>- 뛰어난 가용성, 내구성|\[서비스 개수\]<br/>- 13개<br/><br/>\[주요 서비스\]<br/>- Azure Cosmos DB<br/>- Azure SQL<br/><br/>\[특징\]<br/>- 빠른 속도 보장<br/>- 완전관리형 DB<br/>- 업무용 사용 가능<br/>- 높은 유연성|\[서비스 개수\]<br/>- 6개<br/><br/>\[주요 서비스\]<br/>- Firebase 데이터베이스<br/>- Cloud SQL<br/><br/>\[특징\]<br/>- 데이터 실시간 동기화<br/>- 완전관리형 DB<br/>- 기기간 공동작업 지원<br/>- 강력한 보안성|
|<center><b>AI</b></center>|\[서비스 개수\]<br/>- 25개<br/><br/>\[주요 서비스\]<br/>- Amazon SageMaker<br/>- Amazon Polly<br/><br/>\[특징\]<br/>- 최대 10배 생산성 향상<br/>- 우수한 성능<br/>- 최적화 알고리즘<br/>- 자동화 기능|\[서비스 개수\]<br/>- 5개<br/><br/>\[주요 서비스\]<br/>- 머신 러닝<br/>- Azure Bot Service<br/><br/>\[특징\]<br/>- 모든 기술 생산성 향상<br/>- 수명 주기 관리 기능<br/>- 오픈소스 프레임워크, 언어에 최고 수준 지원|\[서비스 개수\]<br/>- 13개<br/><br/>\[주요 서비스\]<br/>- Vision AI<br/>- Cloud Translation<br/><br/>\[특징\]<br/>- 뛰어난 확장성<br/>- 간편한 통합<br/>- 경제적인 가격<br/>- 고품질 데이터 제공|

## References

- [Amazon Web Services](https://aws.amazon.com/ko/)
- [Microsoft Azure](https://azure.microsoft.com/ko-kr/)
- [Google Cloud](https://cloud.google.com/?hl=ko)