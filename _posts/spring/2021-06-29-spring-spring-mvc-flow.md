---
title: '[Spring] Spring MVC 흐름 (Spring MVC Flow)'
author: da-nyee
date: 2021-06-30 22:00:17 +0900
categories: [TIL, Spring]
tags: [spring, spring mvc, dispatcherservlet, flow]
---

## Workflow

![SpringMVC_DispatcherServlet_Flow](https://user-images.githubusercontent.com/50176238/123804138-ec84a500-d927-11eb-93cf-35f90aa2fa44.png)

- WAS
    - request, response 객체 생성
- DispatcherServlet
    - doService() 실행
    - doDispatch() 실행
    - getHandler()로 handler 조회
    - getHandlerAdapter()로 HandlerAdapter 조회
    - applyPreHandle()로 Interceptor 실행 👉 false면 바로 종료
    - handle()로 HandlerAdapter 실행 -> Handler(Controller) 호출 -> 비즈니스 로직 실행
    - applyPostHandle()로 Interceptor 실행
    - viewName(논리 이름) -> View(물리 이름) 전환 👉 ViewResolver 실행
    - render()로 View + Model 렌더링
    - triggerAfterCompletion()으로 Interceptor 실행

## References

- DispatcherServlet 내부 코드
- [Spring MVC 처리 과정](https://github.com/binghe819/TIL/blob/master/Spring/MVC/Spring%20MVC%20flow.md)