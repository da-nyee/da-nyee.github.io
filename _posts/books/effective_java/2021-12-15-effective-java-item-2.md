---
title: '[Effective Java] 아이템 2. 생성자에 매개변수가 많다면 빌더를 고려하라'
author: da-nyee
date: 2021-12-15 17:37:38 +0900
categories: [BOOKS, Effective Java]
tags: [books, effective java]
---

## 점층적 생성자 패턴 (Telescoping Constructor Pattern)

- 필수 매개변수만 받는 생성자, 필수 매개변수와 선택 매개변수 1개를 받는 생성자.. 형태로 생성자를 늘려가는 방식이다.
- 확장하기 힘들고, 매개변수 개수가 많아지면 클라이언트 코드를 작성하거나 읽기 어렵다는 단점이 있다.

```java
public class NutritionFacts {

    private final int serving;          // 필수
    private final int servings;         // 필수
    private final int calories;         // 선택
    private final int fat;              // 선택
    private final int sodium;           // 선택
    private final int carbohydrate;     // 선택

    public NutritionFacts(int serving, int servings) {
        this(serving, servings, 0);
    }

    public NutritionFacts(int serving, int servings, int calories) {
        this(serving, servings, calories, 0);
    }

    public NutritionFacts(int serving, int servings, int calories, int fat) {
        this(serving, servings, calories, fat, 0);
    }

    public NutritionFacts(int serving, int servings, int calories, int fat, int sodium) {
        this(serving, servings, calories, fat, sodium, 0);
    }

    public NutritionFacts(int serving, int servings, int calories, int fat, int sodium, int carbohydrate) {
        this.serving = serving;
        this.servings = servings;
        this.calories = calories;
        this.fat = fat;
        this.sodium = sodium;
        this.carbohydrate = carbohydrate;
    }
}

// 사용 예시
NutritionFacts cocaCola = new NutritionFacts(240, 8, 100, 0, 35, 27);
```

<br/>

## 자바빈즈 패턴 (JavaBeans Pattern)

- 매개변수가 없는 생성자로 객체를 생성하고, setter를 호출해서 원하는 매개변수 값을 설정하는 방식이다.
- 객체 1개를 만들려면 메서드 n개를 호출해야 하고, 객체가 완전해지기 전까지는 일관성(consistency)이 무너진 상태에 놓이게 된다는 단점이 있다.
- 또, 클래스를 불변으로 유지할 수 없다는 치명적인 단점도 존재한다.

```java
public class NutritionFacts {

    private final int serving = -1;         // 필수, 기본값 없음
    private final int servings = -1;        // 필수, 기본값 없음
    private final int calories = 0;         // 선택, 기본값 있음
    private final int fat = 0;              // 선택, 기본값 있음
    private final int sodium = 0;           // 선택, 기본값 있음
    private final int carbohydrate = 0;     // 선택, 기본값 있음

    public NutritionFacts() {
    }

    public void setServing(int serving) {
        this.serving = serving;
    }

    public void setServings(int servings) {
        this.servings = servings;
    }

    public void setCalories(int calories) {
        this.calories = calories;
    }

    public void setFat(int fat) {
        this.fat = fat;
    }

    public void setSodium(int sodium) {
        this.sodium = sodium;
    }

    public void setCarbohydrate(int carbohydrate) {
        this.carbohydrate = carbohydrate;
    }
}

// 사용 예시
NutritionFacts cocaCola = new NutritionFacts();
cocaCola.setServing(240);
cocaCola.setServings(8);
cocaCola.setCalories(100);
cocaCola.setSodium(35);
cocaCola.setCarbohydrate(27);
```

<br/>

## 빌더 패턴 (Builder Pattern)

- 점층적 생성자 패턴의 <b>안전성</b>과 자바빈즈 패턴의 <b>가독성</b>을 겸비했다.
- 클라이언트는 필수 매개변수만 받는 생성자 또는 정적 팩터리 메서드를 호출해서 빌더 객체를 얻고, 이 객체가 제공하는 일종의 setter로 원하는 선택 매개변수를 설정한다. 마지막에는 매개변수가 없는 build()를 호출해서 우리에게 필요한 객체를 얻는다. 해당 객체는 보통 불변이다.
- 빌더의 setter는 빌더 자신을 반환한다. 그래서 연쇄적으로 호출할 수 있다.
- 이를 <b>메서드 호출이 흐르듯 연결된다</b>고 하여 <b>플루언트 API(fluent API)</b> 또는 <b>메서드 연쇄(method chaining)</b>라 한다.
- 한편, 객체를 만들기에 앞서 빌더부터 만들어야 한다는 단점이 있다.
- 또한 빌더 생성 비용이 크지는 않지만 성능에 민감한 상황에서는 영향을 끼칠 수 있고, 코드가 장황해서 매개변수가 4개 이상은 돼야 값어치를 한다는 단점도 존재한다.

```java
public class NutritionFacts {

    private final int serving;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    private NutritionFacts(Builder builder) {
        serving = builder.serving;
        servings = builder.servings;
        calories = builder.calories;
        fat = builder.fat;
        sodium = builder.sodium;
        carbohydrate = builder.carbohydrate;
    }

    public static class Builder {
        
        // 필수
        private final int serving;
        private final int servings;
    
        // 선택
        private int calories = 0;
        private int fat = 0;
        private int sodium = 0;
        private int carbohydrate = 0;

        public Builder(int serving, int servings) {
            this.serving = serving;
            this.servings = servings;
        }

        public Builder calories(int calories) {
            this.calories = calories;
            return this;
        }

        public Builder fat(int fat) {
            this.fat = fat;
            return this;
        }

        public Builder sodium(int sodium) {
            this.sodium = sodium;
            return this;
        }

        public Builder carbohydrate(int carbohydrate) {
            this.carbohydrate = carbohydrate;
            return this;
        }

        public NutritionFacts build() {
            return new NutritionFacts(this);
        }
    }    
}

// 사용 예시
NutritionFacts cocaCola = new NutritionFacts.Builder(240, 8)
        .calories(100)
        .sodium(35)
        .carbohydrate(27)
        .build();
```