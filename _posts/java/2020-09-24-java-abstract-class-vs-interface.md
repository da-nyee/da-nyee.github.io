---
title: '[Java] 추상 메서드 vs 인터페이스 (Abstract Class vs Interface)'
author: da-nyee
date: 2020-09-24 01:49:08 +0900
categories: [TIL, Java]
tags: [java, abstract class, interface]
---

## Introduction

비슷한 듯 다른 추상 메서드와 인터페이스.<br/>
헷갈리는 두 개념을 제대로 정의해보자.

## Abstraction

- `선언부(어떤 것이 동작하는지)는 보여주고`, `내부 구현부(어떻게 동작하는지)는 숨기는` 형태.
- 추상 메서드와 인터페이스 둘 다 `추상화`를 위해 사용된다.

## Abstract Class

- `추상 메소드(abstract method)가 하나 이상 포함`된 경우 or `abstract로 정의`된 경우

❓ <ins>추상 클래스는 추상 메소드를 최소한 하나라도 가져야 할까?</ins><br/>
👉 NOPE.<br/>
👉 추상 클래스는 추상 메소드를 가지지 않아도 상관없다.<br/>
👉 단, 추상 메소드를 하나라도 가지는 클래스는 추상 클래스가 되어야 한다.<br/><br/>

- 필드, 생성자, 추상 메소드를 가질 수 있다.
- `생성자를 가지기` 때문에 `객체화가 가능`하다.

❓ <ins>추상 클래스도 생성자를 가질 수 있을까?</ins><br/>
👉 YES.<br/>
👉 추상 클래스의 생성자는 일반 클래스에 필요한 어떤 제약을 줄 때 사용한다.<br/>
👉 추상 클래스의 생성자는 하위 클래스의 생성자에서 `super()`를 통해 부르고 초기화 시킨다.

```
public abstract class AbstractClass {

    // Field
    private String name;
    private String id;

    // Constructor 
    public AbstractClass(String name, String id){
      this.name = name;
      this.id = id;
    }

    // Abstract Method
    public method() { }
}

public class RealClass extends AbstractClass {

    private String number;

    public RealClass(String name, String id, String number) {
      super(name, id);
      this.number = number;
   }
}
```
<br/>

- 추상 클래스는 `상속(extends)`된다.
- 상속은 `is a kind of(~의 한 종류)`라는 의미를 가진다.

👉 ex. 뽀로로 is a kind of 펭귄 == 뽀로로는 펭귄의 한 종류이다.<br/>
👉 뽀로로 == 하위 클래스, 펭귄 == 상위 클래스가 된다.

## Interface

- `모든 메소드`가 `추상 메소드`인 경우
- 추상 클래스보다 한 단계 더 추상화된 클래스라고 볼 수 있다.
- `상수(static final)`과 `추상 메소드(abstract method)`의 집합이다.
- `생성자를 가질 수 없기` 때문에 `객체화가 불가능`하다.<br/><br/>

❗ <ins>Java 8부터 지원되는 사항</ins><br/>
👉 인터페이스에 `디폴트 메소드(default method)`를 지원한다.<br/><br/>

❗ <ins>디폴트 메소드의 목적 및 특징</ins><br/>
👉 `기존 인터페이스의 기능을 확장`한다.<br/>
👉 디폴트 메소드 내부에 구현체 공통 기능을 작성함으로써 반복되는 코드 작성을 줄여준다.<br/>
👉 어떤 기능 추가 등 변화가 생기면, 디폴트 메소드 내부만 수정하면 되고 구현체는 수정할 필요가 없다.<br/>
👉 디폴트 메소드를 사용하기 위해서는 `default` 키워드를 꼭 붙여줘야 한다.
```
interface Printable {

    public abstarct void paper();

    // Default Method: 실행 내용까지 있음
    public default void setPrint(boolean color){
      if(color){
        System.out.println("컬러 출력");
      }
      else {
        System.out.println("흑백 출력");
      }
   }
}
```
<br/>

- `인터페이스는 다중 상속`을 지원한다.
- 따라서, 구현체에 여러 개의 인터페이스를 구현할 수 있다.

👉 `클래스는 단일 상속`을 지원한다. 다중 상속은 지원하지 않는다.<br/><br/>

- 상수는 `public static final`, 추상 메소드는 `public abstract`를 생략해도 된다.
- 컴파일러가 컴파일 시 자동으로 생성해준다.<br/>

![java_abstract_class_vs_interface_1](https://user-images.githubusercontent.com/50176238/94044910-bdc22700-fe09-11ea-88b8-1eaeb87b96d3.PNG)
<br/><br/>

- 인터페이스는 `구현(implements)`된다.
- 구현은 `be able to(~할 수 있는)`이라는 의미를 가진다.
- 인터페이스는 네이밍 규칙을 가지고 있는데, 보통 __able 형태로 짓는다.
- 인터페이스 구현체도 네이밍 규칙이 있다. 보통 __Impl 형태로 짓는다.

## extends, implements Keywords

- `extends`: 클래스 ⬅ 클래스, 인터페이스 ⬅ 인터페이스 상속
- `implements`: 인터페이스 ⬅ 클래스 상속<br/>

![java_abstract_class_vs_interface_2](https://user-images.githubusercontent.com/50176238/94044962-c87cbc00-fe09-11ea-82f5-c40c157f3801.PNG)

## Same Features

- 선언부만 있고 구현부는 없는 추상 메소드를 가진다.
- 추상 클래스 혹은 인터페이스를 상속받은 자식 클래스에서 추상 메소드를 구현한다.
- 결국, `자식 클래스`에서 `무언가 반드시 구현하도록 위임`해야할 때 사용한다.

## Different Features

- `추상 클래스는 단일 상속`, `인터페이스는 다중 상속`이 가능하다.<br/>

![java_abstract_class_vs_interface_3](https://user-images.githubusercontent.com/50176238/94045001-d3cfe780-fe09-11ea-9c34-99411d446d55.PNG)
<br/><br/>

- 추상 클래스의 목적은 상속을 받아 `기능을 확장`시키는 것이다. (부모의 유전자를 물려받는다.)
- 인터페이스의 목적은 구현하는 모든 클래스에 대해 `특적 메소드가 반드시 존재하도록 강제`하는 것이다. (부모로부터 유전자를 물려받는 게 아니다. 사교적으로 필요에 따라 결합하는 관계이다.)<br/><br/>

```
/**
* 동물 추상 클래스
*/
@Getter @Setter
public abstract class Animal {

	  private String name;
	  private String leg;

	  /* Abstract method */
	  abstract void run();
}

/**
* 탈 수 있는(Ridable) 인터페이스
*/
public interface Ridable {

	  /**
	  * Abstract method 
	  * @riderName
	  */
	  void youCanRide(String riderName);
}

/**
* 추상클래스를 상속 받고 인터페이스를 구현한 Horse 클래스
*/
public class Horse extends Animal implements Ridable {

	  @Override
	  public void run() {
		  System.out.println(this.getName()+"은(는) 네 발로 뛴다.");
	  }
  
	  @Override
	  public void ridable(String riderName) {
		  System.out.println(this.getName()+"은(는) "+riderName+"을(를) 태울 수 있다.");
	  } 
}

/**
* 추상클래스를 상속 받은 Kangaroo 클래스
*/
public class Kangaroo extends Animal {

	  @Override
	  public void run() {
		  System.out.println(this.getName()+"은(는) 네 발로 뛴다.");
	  }
}

public class MainClass {

	  public static void main(String[] args) {
		  Horse horse = new Horse();
          Kangaroo kangol = new Kangaroo();
    
		  /* 상속받은 말의 속성 */
		  horse.setName("얼룩말");
		  horse.setLeg("다리 4개");
    
   	      /* 상속받은 캥거루의 속성 */
		  kangol.setName("캥거루");
		  kangol.setLeg("다리 4개");
    
		  /* 말 구현체의 메서드 호출 */
   	      horse.run();
		  horse.ridable("BAEKJH");
    
	      /* 캥거루 구현체의 메서드 호출 */
		  kangol.run();
	  }
}
```
- 추상 메소드: `모든 동물`은 `뛴다(run)`.
- 인터페이스: `일부 동물`은 `탈 수 있다(ridable)`.

👉 말은 뛰고(run) 탈 수 있다(is ridable).<br/>
👉 캥거루는 뛰지만(run) 탈 수 없다(is not ridable).

## References

- [Difference between Abstract Class and Interface in Java](https://www.geeksforgeeks.org/difference-between-abstract-class-and-interface-in-java/)
- [[01] 추상클래스와 인터페이스의 차이가 뭐죠?](https://cbw1030.tistory.com/47)
- [자바 인터페이스와 추상클래스](https://medium.com/webeveloper/%EC%9E%90%EB%B0%94-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EC%99%80-%EC%B6%94%EC%83%81%ED%81%B4%EB%9E%98%EC%8A%A4-6eecbe5d6350)