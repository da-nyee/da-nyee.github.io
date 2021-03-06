---
title: '[Tecoble] 웹 MVC 각 컴포넌트 역할'
author: da-nyee
date: 2021-05-17 00:29:39 +0900
categories: [EDUCATION, Tecoble]
tags: [tecoble, web mvc, components, role]
---

> 제가 작성한 원글은 [여기서](https://woowacourse.github.io/tecoble/post/2021-04-26-mvc/) 확인하실 수 있습니다.

<br/>

개발을 하다보면 여러 디자인 패턴을 마주하게 된다. 그 중 가장 자주 보는 디자인 패턴은 `MVC 패턴`일 것이다.
MVC 패턴의 각 컴포넌트(Model, View, Controller)는 담당하는 역할이 있다. 해당 패턴을 사용하기 앞서 어떤 컴포넌트가 무슨 역할을 수행하는지 이해하는 것이 중요하다.<br/>
이번 포스트에서는 `웹 MVC 컴포넌트 역할`을 정리하려 한다. 먼저 MVC가 무엇인지 알아보고, 이어서 각 컴포넌트의 역할을 차례대로 설명한다.<br/>

<br/>

## MVC란?
`MVC(Model-View-Controller)`는 어플리케이션을 세 가지 역할로 구분한 개발 방법론이다.<br/>
사용자가 입력을 담당하는 View를 통해 요청을 보내면 해당 요청을 Controller가 받고, Controller는 Model을 통해 데이터를 가져오고, 해당 데이터를 바탕으로 출력을 담당하는 View를 제어해서 사용자에게 전달한다.
MVC 패턴을 사용하면 Model과 View가 다른 컴포넌트들에 종속되지 않아 변경에 유리하다는 장점을 가질 수 있다.<br/>

<br/>

## 모델(Model)
어플리케이션이 `무엇`을 할 것인지 정의한다. `내부 비즈니스 로직을 처리하기 위한 역할`을 한다.
즉, 데이터 저장소(ex. DB)와 연동하여 사용자가 입력한 데이터나 사용자에게 출력할 데이터를 다룬다.
특히, 여러 개의 데이터 변경 작업(ex. 추가, 변경, 삭제)를 하나의 작업으로 묶은 트랜잭션을 다루는 일도 한다.<br/>
Model은 다른 컴포넌트들에 대해 알지 못한다. 자기 자신이 무엇을 수행하는지만 알고 있다.<br/>

아래 LottoNumber 클래스는 1~45 사이의 로또 번호를 생성하는 Model이다. 해당 범위를 넘어서면 IllegalArgumentException이 발생한다.<br/>

```java
public class LottoNumber {
    private static final int MIN_NUMBER_RANGE = 1;
    private static final int MAX_NUMBER_RANGE = 45;
    private static final String ERROR_INVALID_NUMBER_RANGE = "[ERROR] 로또 번호는 1~45 사이로 입력해주세요.";

    private final int number;

    public LottoNumber(int number) {
        validateNumberRange(number);
        this.number = number;
    }

    private void validateNumberRange(int number) {
        if (number < MIN_NUMBER_RANGE || number > MAX_NUMBER_RANGE) {
            throw new IllegalArgumentException(ERROR_INVALID_NUMBER_RANGE);
        }
    }
}
```

<br/>

## 뷰(View)
최종 사용자에게 무엇을 `화면(UI)`로 보여준다. 화면에 `무엇을 보여주기 위한 역할`을 한다.
즉, 모델이 처리한 데이터나 그 작업 결과를 가지고 사용자에게 출력할 화면을 만든다. 만든 화면은 웹 브라우저가 출력한다.<br/>
View 역시도 다른 컴포넌트들에 대해 알지 못한다. 자기 자신이 무엇을 수행하는지만 알고 있다.<br/>

아래 OutputView 클래스는 로또 어플리케이션 화면을 출력하는 View이다. 어플리케이션 실행에 따라 화면이 다르게 나타난다.<br/>

```java
public class OutputView {
    private static final String MESSAGE_LOTTO_TICKET_COUNT_FORMAT = "수동으로 %d장, 자동으로 %d개를 구매했습니다.";
    private static final String MESSAGE_PROFIT_FORMAT = "총 수익률은 %.2f입니다.";

    public void printLottoTicketsCount(long manualCount, long autoCount) {
        System.out.printf((MESSAGE_LOTTO_TICKET_COUNT_FORMAT) + "%n", manualCount, autoCount);
    }

    public void printProfit(Profit profit) {
        System.out.printf((MESSAGE_PROFIT_FORMAT) + "%n", profit.toDouble());
    }

    public void printError(Exception e) {
        System.out.println(e.getMessage());
    }
}
```

<br/>

## 컨트롤러(Controller)
Model과 View 사이에 있는 컴포넌트이다. Model이 데이터를 `어떻게 처리할지 알려주는 역할`을 한다.
클라이언트의 요청을 받으면 해당 요청에 대한 실제 업무를 수행하는 Model을 호출한다. 클라이언트가 보낸 데이터가 있다면, 모델을 호출할 때 전달하기 쉽게 적절히 가공한다.
Model이 업무 수행을 완료하면 그 결과를 가지고 화면을 생성하도록 View에 전달한다. 즉, 클라이언트의 요청에 대해 Model과 View를 결정하여 전달하는 일종의 조정자로서의 일을 한다.<br/>
Controller는 다른 컴포넌트들에 대해 알고 있다. 자기 자신 외에 Model과 View가 무엇을 수행하는지 알고 있다.<br/>

아래 LottoController 클래스는 로또 어플리케이션 흐름을 제어하는 Controller이다.
InputView를 통해 사용자로부터 입력을 받고, Model을 통해 내부 비즈니스 로직을 처리하고, OutputView를 통해 사용자에게 화면을 출력한다.<br/>

```java
public class LottoController {
    private final InputView inputView = InputView.getInstance();
    private final OutputView outputView = OutputView.getInstance();

    public void run() {
        Money budget = createBudgetMoney();
        Quantity quantity = createQuantity(budget);
        List<LottoTicket> lottoTickets = createLottoTickets(quantity.manual(), quantity.auto());
        WinningLotto winningLotto = createWinningLotto();

        Statistics statistics = new Statistics(winningLotto, lottoTickets);
        Profit profit = new Profit(budget, statistics.getReward());

        outputView.printStatistics(statistics);
        outputView.printProfit(profit);
    }
}
```

<br/>

## MVC 패턴을 사용하며 느낀 장점
레벨 1에서 콘솔 어플리케이션으로 구현했던 체스를 Spark 기반의 웹 어플리케이션으로 변경하는 미션을 진행했다.
또한, 레벨 2에서 Spark 기반의 웹 어플리케이션을 Spring 기반의 웹 어플리케이션으로 변환하는 미션도 진행했다.<br/>
두 미션을 수행하기 전에는 프로그램 실행 환경이 아예 바뀌는 것에 대한 두려움이 있었다. 그러나, 미션을 수행하며 Model은 그대로 사용하고 Controller와 View만 새로 구현하면 되는 걸 깨달았다.<br/>
Model은 내부 비즈니스 로직을 가지기 때문에 보통 개발할 때 구현에 가장 오랜 시간이 걸린다.
이런 Model을 그대로 활용하니까 코드를 크게 변경하지 않아도 금세 새로운 환경에서 작동하는 어플리케이션을 만들 수 있었다.<br/>

<br/>

## MVC 패턴을 사용하며 느낀 단점
소프트웨어를 처음에 설계할 때 시간이 어느정도 소요되는 걸 느꼈다.
Model과 View가 다른 컴포넌트들에 독립적이게 만드는 것이 생각보다 어려웠다.
또한, Model을 탄탄하게 설계해야 변경에 유연한 프로그램을 개발할 수 있다는 걸 깨달았다.<br/>

<br/>

## References

- [MVC 디자인 패턴](https://opentutorials.org/course/697/3828)
- [[아키텍처 패턴] MVC 패턴이란?](https://medium.com/@jang.wangsu/%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4-mvc-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80-1d74fac6e256)
- [MVC 컴포넌트 역할](https://dorothy-koo.gitbooks.io/extra-studies/content/mvc-c544-d0a4-d14d-ccd0.html)
- [MVC Design Pattern](https://www.geeksforgeeks.org/mvc-design-pattern/)