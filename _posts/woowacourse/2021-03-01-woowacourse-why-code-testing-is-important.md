---
title: '[우아한테크코스] 코드 테스트가 중요한 이유 (Why code testing is important)'
author: da-nyee
date: 2021-03-01 23:45:40 +0900
categories: [EDUCATION, Woowacourse]
tags: [woowacourse, code testing]
---

## Introduction

- 로또 2단계를 진행하다가 로또 순위를 반환하는 메소드에 버그가 있는 걸 발견했다.
- 로또에서 2등과 3등은 `보너스 볼 유무`로 결정되는데, 이에 상관 없이 2등으로 결과가 나타나기도 했다.
- 로또 1단계에서 단위 테스트를 1등, 2등, 상금이 없는 경우만 했는데 이게 문제였다. `경계값 테스트`를 할 생각이었다면 3등인 경우도 했어야 됐다.
- 해당 버그를 어떻게 해결했는지와 해당 버그를 해결하면서 느낀점을 간단하게 작성해본다!

## Issue Raising

- 프로덕션 코드에서
    - if 문에서 2등과 3등을 확실하게 거르지 않은 것과
    - 스트림에서 `findAny()`를 사용하여 결과가 애매하게 나오도록 만든 게 문제였다.
- 테스트 코드에서
    - 경계값을 전부 테스트하지 않은 게 문제였다.

```
public static Rank from(long match, boolean hasBonus) {
    if (match == SECOND_MATCH && hasBonus) {
        return Rank.SECOND;
    }
    return Arrays.stream(Rank.values())
            .filter(item -> item.getMatch() == match)
            .findAny()
            .orElse(NOTHING);
}
```
```
@DisplayName("1등 상금 리턴 테스트")
@Test
public void compareFirstRewardTest() {
    Rank rank = Rank.from(6, false);

    assertThat(rank.getReward()).isEqualTo(new Money(2_000_000_000));
}

@DisplayName("2등 상금 리턴 테스트")
@Test
public void compareSecondRewardTest() {
    Rank rank = Rank.from(5, true);

    assertThat(rank.getReward()).isEqualTo(new Money(30_000_000));
}

@DisplayName("상금이 없는 경우")
@Test
public void compareNoRewardTest() {
    Rank rank = Rank.from(1, false);

    assertThat(rank.getReward()).isEqualTo(new Money(0));
}
```

## Bug Fix

- 프로덕션 코드에서
    - if 문의 조건식을 수정하여 2등과 3등을 확실하게 걸렀고,
    - 스트림에서 `findFirst()`를 사용하여 결과가 정확하게 나오도록 만들었다.
- 테스트 코드에서
    - 경계값을 전부 테스트했고,
    - 하는 김에 모든 경우를 테스트하여 코드의 신뢰성을 높였다.

```
public static Rank from(long match, boolean hasBonus) {
    if (match == SECOND_MATCH) {
        return decideSecondOrThird(hasBonus);
    }
    return Arrays.stream(Rank.values())
            .filter(rank -> rank.isSameMatch(match))
            .findFirst()
            .orElse(NOTHING);
}

private static Rank decideSecondOrThird(boolean hasBonus) {
    if (hasBonus) {
        return SECOND;
    }
    return THIRD;
}
```
```
@DisplayName("1등 상금 리턴 테스트")
@Test
public void compareFirstRewardTest() {
    Rank rank = Rank.from(6, false);

    assertThat(rank.getReward()).isEqualTo(new Money(2_000_000_000));
}

@DisplayName("2등 상금 리턴 테스트")
@Test
public void compareSecondRewardTest() {
    Rank rank = Rank.from(5, true);

    assertThat(rank.getReward()).isEqualTo(new Money(30_000_000));
}

@DisplayName("3등 상금 리턴 테스트")
@Test
public void compareThirdRewardTest() {
    Rank rank = Rank.from(5, false);

    assertThat(rank.getReward()).isEqualTo(new Money(1_500_000));
}

@DisplayName("4등 상금 리턴 테스트")
@Test
public void compareFourthRewardTest() {
    Rank rank = Rank.from(4, false);

    assertThat(rank.getReward()).isEqualTo(new Money(50_000));
}

@DisplayName("5등 상금 리턴 테스트")
@Test
public void compareFifthRewardTest() {
    Rank rank = Rank.from(3, false);

    assertThat(rank.getReward()).isEqualTo(new Money(5_000));
}

@DisplayName("상금이 없는 경우")
@Test
public void compareNoRewardTest() {
    Rank rank = Rank.from(1, false);

    assertThat(rank.getReward()).isEqualTo(new Money(0));
}
```

## Conclusion

- 테스트를 꼼꼼하게 하자!
- 경계값 테스트를 할 때, 어디까지가 경계인지 잘 체크하자!