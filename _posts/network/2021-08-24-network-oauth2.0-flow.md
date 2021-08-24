---
title: '[Network] OAuth 2.0 흐름 (OAuth 2.0 Flow)'
author: da-nyee
date: 2021-08-24 13:52:31 +0900
categories: [TIL, Network]
tags: [network, oauth 2.0, flow]
---

## Workflow

![OAuth_Flow](https://user-images.githubusercontent.com/50176238/130556864-6c18d77a-c577-46ed-a3c4-aaa5b2d5c21f.png)

1. 사용자는 Pick-Git에서 Github Login에 필요한 자원에 접근한다.
2. Github Login URL(client_id, redirect_url, scope 포함)을 응답한다.
3. 사용자는 로그인 버튼을 클릭하여 Github Login 페이지로 이동한다.
4. Github은 사용자에게 Login URL에 명시한 scope에 대한 정보 제공 동의 허용 여부를 물어본다.
5. 사용자가 동의하고 로그인에 성공한다.
6. Github은 사용자에게 Authorization Code를 발급한다.
7. 사용자는 Pick-Git에게 Authorization Code를 전달한다.
8. Pick-Git은 Github에게 client_id, client_secret, Authorization Code를 전송한다.
9. Github은 전달받은 데이터를 검증하고, Pick-Git에게 Github Access Token을 발급한다.
10. Pick-Git은 Github Access Token을 이용하여 Github에게 해당 사용자 프로필 정보를 요청한다.
11. Github은 해당 Access Token을 검증하고, Pick-Git에게 사용자 프로필 정보를 응답한다.
12. Pick-Git은 사용자 이름을 payload로 Pick-Git Access Token을 생성하고,<br/>
이를 { Pick-Git Access Token : Github Access Token } 형식으로 저장하고,<br/>
사용자에게 Pick-Git Access Token을 전달한다.

<br/>

## References

- [OAuth2 동작 흐름에 대해서 알아보자](https://woodcock.tistory.com/17)
- [OAuth 2.0](https://github.com/binghe819/TIL/blob/master/Network/OAuth%202.0/OAuth2.0.md)