---
title: '[Java] Gradle Dependency Configurations'
author: da-nyee
date: 2021-06-21 18:05:10 +0900
categories: [TIL, Java]
tags: [java, gradle, dependency, configurations]
---

## compileClasspath
- 컴파일 시에 필요한 class path

## runtimeClasspath
- 런타임 시에 필요한 class path
- 보통, compile하면 compile path에도 있고 runtime path에도 있다.

<br/>

![gradle-configurations](https://user-images.githubusercontent.com/50176238/122738296-d38f4a80-d2bc-11eb-9124-f8f89a266c0f.png)

## compileOnly
- 컴파일 시에만 필요하고, 런타임 시에는 불필요한 경우
- 컴파일할 때는 해당 종속성을 사용하고 build 결과물에는 넣지 X
- Advantage? build 결과물의 크기가 작아진다는 장점 O
- compileClassPath에만 dependency를 두는 것
- ex. Lombok -> 컴파일 시에 lombok annotation을 보고 getter/setter 등 만들어주고 런타임 시에는 사용하지 X

## runtimeOnly
- 컴파일 시에는 불필요하고, 런타임 시에만 필요한 경우
- Advantage? 컴파일 시간이 빨라진다 -> 컴파일할 때는 사용하지 않기 때문
- runtimeClassPath에만 dependency를 두는 것
- ex. JDBC 관련 -> e.g. h2, mysql
- ex. log 관련 -> log4j

```
예를 들어,

class A {
    B = new B();
}

class B {
    int dani() {
	    return new C().wow();
    }
}

만약 A 클래스를 사용한다고 가정하면,
컴파일 시점에 B 클래스 의존 O
런타임 시점에 B, C 클래스 의존 O
```

## implementation
- ex. A -> B -> C 와 같은 상황
- 이때 C를 변경하면, B와 C만 recompile 하면 된다 -> B는 C를 직접적으로 의존하고 있기 때문
- A 입장에서는 compileClassPath에 C가 들어가지 X
- 따라서, C가 변경된다고 해도 A를 recompile 할 필요가 X

## api
- 한편, implementation과 동일한 상황에서 api는 A에서 C에 대한 접근이 가능함
- A 입장에서 compileClassPath에 C가 O
- 따라서, C가 변경되면 A 또한 recompile O

> `소비자에게 유출된다` 의미?<br/>

- ex. A -> B -> C에서 api를 사용하는 경우
- A를 사용하는 소비자 입장에서 compileClassPath에 C가 들어오게 되니까 노출된다는 뜻
- 물론 runtimeClassPath에는 C를 사용하니까 노출되는 게 당연함 -> api, implementation 둘 다 !

> compileOnly vs compileOnlyApi<br/>

- compileOnly는 ex. A -> B -> C 경우에 C가 변경되면 A는 recompile 할 필요 X, B랑 C만 recompile 하면 O
- compileOnlyApi는 ex. A -> B -> C 경우에 C가 변경되면 A, B, C 전부 recompile O -> api 가 붙었기 때문에 !

<br/>

## References
- [Gradle dependency configuration : implementation vs api vs runtimeonly vs compileonly](https://stackoverflow.com/questions/47365119/gradle-dependency-configuration-implementation-vs-api-vs-runtimeonly-vs-compil)
- [Gradle 파일 implementation, api, runtimeOnly, compileOnly ... 등에 대해](https://github.com/Be-poz/TIL/blob/master/Spring/Gradle%20%ED%8C%8C%EC%9D%BC%20implementation%2C%20api%2C%20runtimeOnly%2C%20compileOnly%20...%20%EB%93%B1%EC%97%90%20%EB%8C%80%ED%95%B4.md)