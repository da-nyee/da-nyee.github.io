---
title: '[Design Pattern] SOLID 원칙 (SOLID Principles)'
author: da-nyee
date: 2021-02-15 23:57:42 +0900
categories: [TIL, Design Pattern]
tags: [design pattern, solid, principles]
---

## SOLID Principles?

- 로버트 C. 마틴이 명명한 <b>객체지향 프로그래밍 및 설계의 기본 원칙</b>이다.
- 프로그래머가 <b>유지보수가 쉽고</b>, <b>확장성이 있는</b> 프로그램을 개발하려 할 때 이 원칙들을 적용할 수 있다.

## SRP (Single Responsibility Principle)

- 단일 책임 원칙
- <b>한 객체는 단 하나의 책임만</b> 가져야 한다.
- 객체지향적으로 설계할 때는 <b>응집도는 높게</b>, <b>결합도는 낮게</b> 설계해야 한다.

> 응집도는 모듈 내부의 기능적인 응집 정도이고, 결합도는 모듈과 모듈간의 상호 결합 정도이다.

- 객체에 책임이 많아질수록 객체 내부에서 서로 다른 역할을 수행하는 코드끼리 강하게 결합할 가능성이 높아진다.
- 객체마다 책임을 제대로 나누지 않으면 시스템은 매우 복잡해진다.
- 왜냐하면, 객체가 하는 일에 변경이 있을 때에 해당 기능을 사용하는 부분을 모두 다시 테스트해야 하기 때문이다.
- 그러므로, 한 객체가 단 하나의 일만 가지도록 분배하면 시스템에 변화가 생겨도 그 영향을 최소화할 수 있다.

### Example

- <b>Calculator</b> 객체는 덧셈, 뺄셈, 곱셈, 나눗셈만 할 수 있다. 👉 사칙연산에 대한 책임만 가지고 있다.
- 이때, 계산기에 알람을 추가한다.
- 만약, <b>Calculator</b>의 기능으로 <b>alarm()</b>을 추가하면 SRP에 위배된다.
- 따라서, <b>Calculator</b> 객체 외에 <b>Alarm</b> 객체를 생성해줘야 한다.

![SRP](https://user-images.githubusercontent.com/50176238/107949212-45d2e400-6fd8-11eb-8349-b018e3e3d1c0.PNG)

## OCP (Open-Closed Principle)

- 개방-폐쇄 원칙
- <b>확장에 대해서는 개방적</b>, <b>변경에 대해서는 폐쇄적</b>이어야 한다.
- 기존 코드에 기능을 추가하되(open) 기존 코드는 변경하지 않는(close) 설계가 되어야 한다.
- 여러 객체에서 사용하는 동일한 기능을 <b>인터페이스</b>에 정의하는 방법이 있다. 👉 <b>캡슐화</b>

### Example

- <b>Animal</b> 인터페이스를 구현하는 Dog, Cat, Bird 클래스는 <b>cry()</b>를 재정의한다.
- 이처럼 캡슐화를 하면, 동물이 추가돼도 <b>cry()</b>를 호출하는 부분은 수정하지 않아도 쉽게 확장할 수 있다.

![OCP](https://user-images.githubusercontent.com/50176238/107950873-bed33b00-6fda-11eb-94dd-076ab7e36918.PNG)

- 클라이언트는 <b>Animal</b> 인터페이스에서 정의한 <b>cry()</b>만 호출하면 된다.

```
public class Client {
    public static void main(String args[]){
        Animal dog = new Dog();
        Animal cat = new Cat();
        
        dog.cry();
        cat.cry();
    }
}
```

## LSP (Liskov Substitution Principle)

- 리스코프 치환 원칙
- <b>자식 클래스는 최소한 부모 클래스에서 가능한 행위를 수행할 수 있어야</b> 한다.
- 자식 클래스는 부모 클래스의 역할을 대체할 수 있어야 한다. 👉 클래스들의 행위는 일관된다.
- <b> 자식 클래스는 부모 클래스의 책임을 무시하거나 재정의하지 않고, 확장만 수행</b>한다.
- 오버라이드(재정의)는 가급적 피한다.

> 📚 우아한테크코스 SOLID 스터디에서 일부 발췌한 내용입니다.<br/>
> 💭 오버라이드를 못하면 상속의 의미가 없지 않나요?<br/>
> 💬 오버라이드 자체를 막는 게 아닙니다. 해당 책임이나 다른 예외처리를 하지 않는 게 문제예요.<br/>
> 부모 클래스의 책임과 예외처리를 해결하면, 오버라이드도 가능합니다.<br/>

## ISP (Interface Segregation Principle)

- 인터페이스 분리 원칙
- <b>클래스는 자신이 사용하지 않는 인터페이스는 구현하지 말아야</b> 한다.
- 하나의 거대한 인터페이스보다 <b>여러 개의 구체적인 인터페이스가 낫다.</b>

> SRP가 객체의 단일 책임을 뜻한다면, ISP는 인터페이스의 단일 책임을 뜻한다.

### Example

- <b>Phone</b>에는 call, text, alarm, calculate 기능이 있다.
- <b>3GPhone</b>과 <b>4GPhone</b>은 해당 기능들을 사용한다.
- 만약, <b>Phone</b> 인터페이스에 해당 기능들을 정의하면 ISP에 위배된다.

![ISP_NO](https://user-images.githubusercontent.com/50176238/107953913-1d9ab380-6fdf-11eb-996a-8a3b153aecda.PNG)

- <b>Phone</b> 인터페이스에 모든 함수를 정의하지 않고, <b>Call</b>, <b>Text</b>, <b>Alarm</b>, <b>Calculator</b> 인터페이스에 각 함수를 정의하고 각 클래스에서 각각의 인터페이스를 구현하도록 설계하면 ISP를 만족한다.
- 결국, 각 인터페이스의 각 메소드가 서로 영향을 미치지 않게 된다.
- 따라서, 클래스는 자신이 사용하지 않는 메소드에 대해서 영향력이 줄어들게 된다.

![ISP_YES](https://user-images.githubusercontent.com/50176238/107954668-16c07080-6fe0-11eb-9538-f7608edc25ee.PNG)

## DIP (Dependency Inversion Principle)

- 의존 역전 원칙
- <b>의존 관계는 변화하기 어렵거나 변화가 거의 없는 것과 맺어야</b> 한다.
- 객체들이 서로 정보를 주고 받을 때 의존 관계가 형성되는데, 이때 객체들은 추상성이 낮은 클래스보다 <b>추상성이 높은 클래스와 의존 관계를 맺어야</b> 한다.
- 일반적으로 <b>인터페이스</b>를 활용하면 이 원칙을 준수할 수 있다. 👉 <b>캡슐화</b>

### Example

- <b>Client</b> 객체는 Dog, Cat, Bird의 <b>cry()</b>에 직접 접근하지 않는다.
- <b>Animal</b> 인터페이스의 <b>cry()</b>를 호출하면서 DIP를 만족한다.

![DIP](https://user-images.githubusercontent.com/50176238/107955243-e927f700-6fe0-11eb-984b-6df915ede5e7.PNG)

## References

### SOLID

- [SOLID (객체 지향 설계)](https://ko.wikipedia.org/wiki/SOLID_(%EA%B0%9D%EC%B2%B4_%EC%A7%80%ED%96%A5_%EC%84%A4%EA%B3%84))
- [[디자인패턴] SOLID 원칙](https://victorydntmd.tistory.com/291)
- [SOLID 원칙](https://medium.com/@hckcksrl/solid-%EC%9B%90%EC%B9%99-182f04d0d2b)

### Cohesion, Coupling

- [[설계 용어] 응집도(Cohesion)와 결합도(Coupling)](https://medium.com/@jang.wangsu/%EC%84%A4%EA%B3%84-%EC%9A%A9%EC%96%B4-%EC%9D%91%EC%A7%91%EB%8F%84%EC%99%80-%EA%B2%B0%ED%95%A9%EB%8F%84-b5e2b7b210ff)