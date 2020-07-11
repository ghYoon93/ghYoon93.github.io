---
title: 흥미로운 시간
categories: TIL
tags: TDD
---

**어떤 금액(주가)을 어떤 주(주식의 수)에 곱한 금액을 결과로 얻을 수 있어야 한다.**

* $5 + 10CHF = $10
* ~~$5 X 2 = $10~~
* ~~amount를 private으로 만들기~~
* ~~Dollar 부작용(side effect)?~~
* Money 반올림?
* ~~equals()~~
* hashCode()
* Equal null
* Equal object
* ~~5CHF X 2 = 10CHF~~
* Dollar/Franc 중복
* ~~공용 equals~~
* **공용 times**
* ~~Franc과 Dollar 비교하기~~
* ~~통화?~~
* testFrancMultiplication을 지워야할까?



두 times() 구현이 거의 비슷하긴 하지만 아직 완전히 동일하지 않다.

```java
// Franc
Money times(int multiplier) {
    return Money.franc(amount * multiplier);
}

// Dolar
Money times(int multiplier) {
    return Money.dollar(amount * multiplier);
}
```

이 둘을 동일하게 만들기 위한 명백한 방법이 없다. 때로는 전진하기 위해서 물러서야 할 때도 잇는 법이다.

팩토리 메서드를 인라인시키면 어떨까?

```java
// Franc
Money times(int multiplier) {
    // return Money.franc(amount * multiplier);
    return new Franc(amount * multiplier, "CHF");
}

// Dolar
Money times(int multiplier) {
    // return Money.dollar(amount * multiplier);
    return new Dollar(amount * multiplier, "USD");
}
```

Franc에서는 인스턴스 변수 currency가 항상 'CHF'이므로 다음과 같이 쓸 수 있다.

```java
// Franc
Money times(int multiplier) {
    // return new Franc(amount * multiplier, "CHF");
    return new Franc(amount * multiplier, currency);
}

// Dolar
Money times(int multiplier) {
    // return new Dollar(amount * multiplier, "USD");
    return new Dollar(amount * multiplier, currency);
}
```

Franc을 가질지 Money를 가질지는 중요한 사실이 아니다.

우리에겐 깔끔한 코드와, 그 코드가 잘 작동할 거라는 믿음을 주는 테스트 코드들이 있다.

times()를 수정하고 테스트를 돌려서 컴퓨터에게 직접 물어보자.

```java
// Franc
Money times(int multiplier) {
    // return new Franc(amount * multiplier, "CHF");
    return new Money(amount * multiplier, currency);
}
```

Money를 콘크리트 클래스로 바꿔야 한다고 말한다.

```java
class Money{
    Money times(int multiplier) {
        return null;
    }
}

```

빨간 막대가 뜨고 에러 메세지가 객체 주소를 포함하는 것을 볼 수 있다. 어떤 에러인지 명확히 하기 위해 객체 주소를 문자열로 확인하자

```java
// Money
public String toString() {
    return amount + " " + currency;
}
```

테스트 코드를 먼저 작성하는게 맞지만

* toString()은 디버그 출력에만 쓰이기 때문에 이게 잘못 구현됨으로 인해 얻게 될 리스크가 적다.
* 이미 빨간 막대 상태인데 이 상태에서는새로운 테스트를작성하지않는 게 좋을 것 같다.



