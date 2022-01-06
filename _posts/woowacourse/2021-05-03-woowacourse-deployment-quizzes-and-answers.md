---
title: '[우아한테크코스] 배포 퀴즈 및 정답 (Deployment Quizzes and Answers)'
author: da-nyee
date: 2021-05-03 02:13:41 +0900
categories: [EDUCATION, Woowacourse]
tags: [woowacourse, deployment, quizzes, answers]
---

## 1.

### Quiz
브라우저에서 `google.com`를 요청할 때 통신 과정이 어떻게 이루어질까요?

### Answer

1. 브라우저의 URL을 파싱한다.
    - 브라우저는 `google.com` 의 구조를 해석한다.
    - 브라우저는 1) 어떤 프로토콜을 통해 해당 URL에 요청할 것인지 2) 어떤 URL로 요청할 것인지 3) 어떤 포트로 요청할 것인지 분석한다.
    - 명시적으로 `프로토콜` 과 `포트` 를 선언하지 않았다면 브라우저는 설정된 기본값을 이용하여 요청한다.<br/>
    e.g. HTTP는 80 포트, HTTPS는 443 포트
2. HSTS 목록을 조회한다.
    - HSTS(HTTP Strict Transport Security)
    - HTTP는 허용하지 않고, HTTPS를 사용하는 연결만 허용하는 기능이다.
    - 만약 HTTP로 요청이 오면 HTTP 응답 헤더에 `Strict Transport Security` 라는 필드를 포함하여 응답하고, 이를 확인한 브라우저는 해당 서버에 요청할 때 HTTPS만을 통해 통신한다.
    - 그리고 자신의 HSTS 캐시(HSTS 목록)에 해당 URL을 저장한다.
    - 브라우저에서는 이 HSTS 목록 조회를 통해 해당 요청을 HTTPS로 보낼지 판단한다.
    - 만약 HSTS 목록에 해당 URL이 존재하면, 명시적으로 HTTP를 통해 요청한다 해도 브라우저가 이를 HTTPS로 요청한다.
3. URL을 IP 주소로 변환한다.
    - 도메인 주소로는 컴퓨터끼리 통신할 수 없다.
    - 따라서, 이를 인터넷 상에서 컴퓨터가 읽을 수 있는 IP 주소로 변환해야 서로 통신을 할 수 있다.
    - 먼저, 브라우저는 로컬 PC의 hosts 파일과 브라우저 캐시에 해당 URL이 존재하는지 확인한다.
    - 만약 존재하지 않는다면, 도메인 주소를 IP 주소로 변환해주는 DNS(Domain Name System) 서버에 요청하여 해당 URL을 IP 주소로 변환한다.
    - DNS 서버로 요청하는 과정
        - /etc/hosts에 해당 URL의 IP 주소가 있는지 확인한다. /etc/hosts에 해당 IP 주소가 존재하면 이를 응답한다. 존재하지 않으면 로컬(책임) DNS 서버와 통신한다.
        - 로컬 DNS 서버에 해당 URL의 IP 주소를 요청한다. 로컬 DNS 서버에 해당 IP 주소가 존재하면 이를 응답한다. 존재하지 않으면 다른 DNS 서버와 통신한다.
        - 로컬 DNS는 root DNS 서버에 해당 URL의 IP 주소를 요청한다. root DNS 서버에 해당 IP 주소가 존재하면 이를 응답한다. 존재하지 않으면 하위 DNS 서버에 요청하라고 응답한다.
        - 로컬 DNS는 .com 도메인을 관리하는 DNS 서버에 해당 URL의 IP 주소를 요청한다. .com DNS 서버에 해당 IP 주소가 존재하면 이를 응답한다. 존재하지 않으면 하위 DNS 서버에 요청하라고 응답한다.
        - 로컬 DNS는 `google.com` 도메인을 관리하는 DNS 서버에 해당 URL의 IP 주소를 요청한다. google.com DNS 서버는 해당 IP 주소를 응답한다.
        - 로컬 DNS는 응답받은 해당 IP 주소를 캐싱하고 응답한다.
4. 라우터를 통해 해당 서버의 게이트웨이까지 이동한다.
    - IP 주소를 이용하여 해당 서버로 요청을 전송한다.
    - IP 주소로 요청이 가야 하는 것은 알지만 어떻게 가야 하는지 경로는 알 수 없다. 요청이 네트워크를 타고 어떻게 이동할지는 네트워크 장비인 `라우터(router)` 의 라우팅을 통해 이뤄진다.
    - 라우터에서는 `라우팅 테이블(routing table)` 을 바탕으로 요청이 어떤 경로를 통해 가야 할지 경로를 지정한다. 이를 통해 해당 요청은 IP 주소를 찾아간다.
5. ARP를 통해 IP 주소를 MAC 주소로 변환한다.
    - 실질적인 통신을 하기 위해서는 논리 주소 == IP 주소를 물리 주소 == MAC 주소로 변환해야 한다.
    - 이를 위해 해당 네트워크 내에서 ARP를 브로드캐스트(broadcast)한다.
    - 해당 IP 주소를 가지고 있는 노드는 자신의 MAC 주소를 응답한다.
6. 대상 서버와 TCP 소켓을 연결한다.
    - 소켓 연결은 3-way handshake 과정을 통해 이뤄진다.
        - Client `— TCP SYN —>` Server
        - Client `<— TCP SYN + ACK —` Server
        - Client `— TCP ACK —>` Server
    - e.g. HTTPS 요청 👉 암호화 통신을 위한 TLS handshake 과정이 추가적으로 발생한다.

    ![https_request_3_way_handshake_with_tls_shandshake](https://user-images.githubusercontent.com/50176238/116821294-3f561080-abb4-11eb-9b77-4c1f49178d16.png)

7. HTTP(HTTPS) 프로토콜로 요청하고 응답한다.
    - 연결이 확정되고, 해당 페이지 `google.com`을 서버에게 요청한다.
    - 서버에서 해당 요청을 받고, 이 요청을 수락할 수 있는지 검사한다.
    - 서버는 요청에 대한 응답을 생성하여 브라우저에게 전달한다.
8. 브라우저에서 응답을 해석한다.
    - 서버에서 응답한 내용은 HTML, CSS, JavaScript 등으로 이루어져 있다.
    - 브라우저가 이를 해석하여 `google.com` 페이지를 화면에 그려준다.

## 2.

### Quiz

DDoS 공격에는 어떻게 대응하면 좋을까요?

### Answer

1. 공격 대상 영역을 줄인다.
    - 공격을 받을 수 있는 대상 영역을 최소화하여 공격자의 옵션을 제한하고 한 곳에서 보호 기능을 구축한다.
        - 컴퓨팅 리소스를 CDN(Content Delivery Network) 또는 로드 밸런서 뒤에 배치하고, DB 서버와 같은 인프라의 특정 부분에 인터넷 트래픽이 직접 접근하지 못하도록 제한할 수 있다.
        - 방화벽 또는 ACL(Access Control List)를 사용하여 어플리케이션에 도달하는 트래픽을 제어할 수 있다.
2. 규모에 대해 대비한다.
    - 대규모 볼륨의 DDoS 공격을 완화하기 위한 주요 고려 사항 두 가지는 공격을 흡수하고 완화할 수 있는 대역폭 용량(전송 용량)과 서버 용량이다.
    - 대역폭 용량(전송 용량)
        - 어플리케이션을 설계할 땐 호스팅 제공업체가 대량의 트래픽을 처리할 수 있도록 충분한 중복 인터넷 연결을 제공하는지 확인해야 한다.
        - DDoS 공격의 최종 목표는 리소스 또는 어플리케이션의 가용성에 영향을 주는 것이다.
        - 따라서, 이들을 최종 사용자뿐만 아니라 대규모 인터넷 교환망에 가까운 곳에 배치해야 한다.
        - 이렇게 하면 트래픽 볼륨이 증가하더라도 사용자가 어플리케이션에 쉽게 접근할 수 있다.
        - 또, 웹 어플리케이션의 경우에는 CDN 또는 스마트 DNS 확인 서비스를 사용하여 최종 사용자에게 조금 더 가까운 위치에서 콘텐츠를 제공하고, DNS 쿼리를 해결할 수 있는 추가적인 네트워크 인프라 계층을 제공할 수 있다.
    - 서버 용량
        - 대부분의 DDoS 공격은 많은 리소스를 소모하는 볼륨 공격이다.
        - 따라서, 컴퓨팅 리소스를 신속하게 확장 또는 축소할 수 있는 기능이 중요하다.
        - 해당 기능을 위해 더 큰 컴퓨팅 리소스에서 실행한다. 혹은 더 큰 볼륨을 지원하는 광범위한 네트워크 인터페이스 또는 향상된 네트워킹과 같은 기능이 있는 컴퓨팅 리소스에서 실행한다.
        - 또, 로드 밸런서를 사용해서 지속적으로 리소스 간 로드를 모니터링하고 이동하여 하나의 리소스에만 과부하가 걸리지 않도록 하는 방법도 있다.
3. 정상 및 비정상 트래픽을 파악한다.
    - 개별 패킷 자체를 분석하여 지능적으로 합법적인 트래픽만 수용한다.
    - 이를 위해서는 대상이 일반적으로 수신하는 정상 트래픽의 특징을 이해하고 각 패킷을 이 기준과 비교할 수 있어야 한다.
4. 정교한 애플리케이션 공격에 대비하여 방화벽을 배포한다.
    - 어플리케이션 자체의 취약성을 악용하려고 시도하는 SQL 주입 또는 사이트 간 요청 위조와 같은 공격에 대비하려면 WAF(Web Application Firewall)을 사용하는 게 좋다.
    - 정상 트래픽으로 위장하거나 잘못된 IP 또는 예상치 못한 지역에서 수신되는 등의 특징을 갖는 불법적인 요청에 대비하여 사용자 지정 완화 기능을 손쉽게 생성할 수 있어야 한다.
    - 때론 트래픽 패턴을 연구하고 사용자 지정 보호 기능을 생성할 수 있는 경험이 풍부한 엔지니어의 지원을 받는 것도 공격을 완화하는 데 도움이 될 수 있다.

## 3.

### Quiz

현재 서버에서 몇 개의 연결까지 가능한가요?

### Answer

- Java에서는 `max user processes` 만큼 쓰레드 생성이 가능하다.
    - Linux에서는 프로세스 == 쓰레드
    - 따라서, 현재 상태에서는 `15621` 개의 쓰레드를 생성할 수 있다.

<img width="330" alt="max_user_processes" src="https://user-images.githubusercontent.com/50176238/116821344-83491580-abb4-11eb-95f0-f5c252fba583.png">

- Java에서는 `open files` 만큼 소켓 생성이 가능하다.
    - open files는 `프로세스가 가질 수 있는 소켓 포함 파일 개수`이다.
- Java에서는 소켓 생성 제한이 `hard 옵션` 을 따라간다.
    - Java에서 `maxFDLimit = true` 로 디폴트 설정되어 있어 Linux 환경에서 JDK를 실행하면 자동으로 limit 사이즈가 증가한다. 👉 JDK 내부 코드상에서 soft limit 값이 `hard limit` 값으로 업데이트된다.
    - 따라서, 현재 상태에서는 대략 `1000000` 개의 소켓 생성을 할 수 있다.(연결이 가능하다.)

<img width="330" alt="open_files" src="https://user-images.githubusercontent.com/50176238/116821369-9a880300-abb4-11eb-8ae4-74f9fc404cdf.png">

## 4.

### Quiz

생성한 EC2의 스토리지 용량을 재부팅 없이 늘리려면 어떻게 해야할까요?

### Answer

1. AWS EC2 화면에서 볼륨을 수정한다.
    - EC2 → Elastic Block Store → Volumes 으로 이동한다.
    - 조정하고 싶은 볼륨을 클릭하고, Actions → Modify Volume 을 선택한다.
        - 팝업이 띄워지면 사이즈에 늘리고 싶은 용량(e.g. 8GB → 150GB)을 입력한다.
        - 수정하기 버튼을 클릭하고, 예 버튼을 클릭해서 새로운 용량을 반영한다.
    - 여기까지 하면 화면에서 스토리지 용량이 늘어난 걸 확인할 수 있다. 하지만, 시스템에 반영된 건 아니다. 시스템에 반영하기 위해 일부 과정을 더 거쳐야 한다.
2. 파티션 크기를 조정한다.
    - `cloud-guest-utils` 를 설치한다.

    ```bash
    apt install cloud-guest-utils
    ```

    - 파티션 크기를 늘린다.

    ```bash
    growpart /dev/xvda 1
    ```

    - 파티션 크기를 확인한다.

    ```bash
    lsblk
    NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    loop0     7:0    0   91M  1 loop /snap/core/6350
    loop1     7:1    0   18M  1 loop /snap/amazon-ssm-agent/930
    loop2     7:2    0 89.4M  1 loop /snap/core/6818
    loop3     7:3    0 17.9M  1 loop /snap/amazon-ssm-agent/1068
    xvda    202:0    0  150G  0 disk
    └─xvda1 202:1    0  150G  0 part /
    ```
3. 파일 시스템 크기를 조정한다.
    - 파일 시스템 크기를 늘린다.

    ```bash
    resize2fs /dev/xvda1
    ```

    - 파일 시스템 크기를 확인한다.

    ```bash
    df -h
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/xvda1      146G  4.9G  141G   3% /
    ```

## References

- [브라우저에 URL을 입력했을 때 발생하는 일들](https://deveric.tistory.com/97)
- [DDoS 공격이란 무엇입니까?](https://aws.amazon.com/ko/shield/ddos-attack-protection/)
- [Java, max user processes, open files](https://woowabros.github.io/experience/2018/04/17/linux-maxuserprocess-openfiles.html)
- [Resize EBS volume without rebooting in AWS](https://www.fizerkhan.com/blog/posts/resize-ebs-volume-without-rebooting-in-aws)