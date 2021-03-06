---
title: 흥미로운 시간
categories: TIL
tags: TDD
---



**어떤 금액(주가)을 어떤 주(주식의 수)에 곱한 금액을 결과로 얻을 수 있어야 한다.**

* ~~$5 X 2 = $10 (1)~~
* ~~amount를 private으로 만들기 (4)~~
* ~~Dollar 부작용(side effect) (2)?~~
* Money 반올림?
* ~~equals() (3)~~
* hashCode()
* Equal null
* Equal object
* ~~5CHF X 2 = 10CHF (5)~~
* ~~Dollar/Franc 중복 (8)~~
* ~~공용 equals (6)~~
* **공용 times (10)**
* ~~Franc과 Dollar 비교하기 (7)~~ 
* ~~통화 (9)~~
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

이 둘을 동일하게 만들기 위한 명백한 방법이 없다. 때로는 전진하기 위해서 물러서야 할 때도 있는 법이다.

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
* 이미 빨간 막대 상태인데 이 상태에서는새로운 테스트를 작성하지않는 게 좋을 것 같다.



>expected: <10 CHF> but was: <10 CHF>

toString()으로 에러 메세지를 명확히 했지만 여전히 혼란스럽다.

답은 맞았지만 클래스가 다른 것이 원인이다. Franc 대신 Money가 왔다.

문제는 equals() 구현에 있다.

```java
// Money
public boolean equals(Object object) {
    Money money = (Money) object;
    return amount == money.amount
        && getClass().equals(money.getClass());
}
```

지금까지 우리는 객체 개념이 동일한지 검사했다. 하지만 이제 통화 개념을 도입했으니 이제 검사해야할 것은 통화가 같은지 여부이다.

방금 toString() 작성 시 빨간 막대 상태에서 새로운 테스트를 작성하지 않는 것이 좋을 것이라고 했다. 하지만 지금은 실제 모델 코드(equals())를 수정하려고 하는 중이고 테스트 없이는 모델 코드를 수정할 수 없다.

보수적인 방법을 따르자면 변경된 코드를되돌려서 다시 초록 막대 상태로 돌아가야 한다. 그러고 나서 equals()를 위해 테스트를 고치고 구현 코드를 고칠 수 있게되고, 그 후에야 원래 하던일을 다시 할 수 있다.



```java
// Franc
Money times(int multiplier) {
    // return new Money(amount * multiplier, currency);
    return new Franc(amount * multiplier, currency);
}
```

다시 Franc을 반환하도록 바꿨다. 테스트도 원래대로 초록 막대 상태가 되었다.

Franc(10, "CHF")과 Money(10, "CHF")가 서로 같기를 바랬지만, 사실은 그렇지 않다고 보고됐다.

우리는 이 사실을 그대로 테스트로 사용할 수 있다.

```java
public void testDifferentClassEquality() {
    assertTrue(new Money(10,"CHF").equals(new Franc(10, "CHF")));
}
```

이제 equals()코드에서 currency를 비교해보자

```java
// Money
public boolean equals(Object object) {
    Money money = (Money) object;
    // return amount == money.amount
    //         && getClass().equals(money.getClass());
    return amount == money.amount
        && currency().equals(money.currency());
}

// Franc
Money times(int multiplier) {
    // return new Franc(amount * multiplier, currency);
    return new Money(amount * multiplier, currency);
}

// Dollar
Money times(int multiplier) {
    // return new Franc(amount * multiplier, currency);
    return new Money(amount * multiplier, currency);
}
```

이제 다시 Money를 반환해도 테스트를 통과할 수 있게 되었다.

Dollar와 Franc의 times()가 동일해졌으므로 상위 클래스로 올리자.

```java
// Money
Money times(int multiplier) {
    // return new Franc(amount * multiplier, currency);
    return new Money(amount * multiplier, currency);
}
```



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
* ~~공용 times~~
* ~~Franc과 Dollar 비교하기~~
* ~~통화?~~
* testFrancMultiplication을 지워야할까?



**검토**

* 두 times()를 일치시키기 위해 그 메스드들이 호출하는 다른 메서드들을 인라인시킨 후 상수를 변수로 바꿔주었다.
* 단지 디버깅을 위해 테스트 없이 toString()을 작성했다.
* Franc 대신 Money를 반환하는 변경을 시도한 뒤 그것이 잘 작동할지를 테스트가 말하도록 했다.
* 실험해본 것을 뒤로 되돌리고 또 다른 테스트를 작성했다. 테스트를 작동했더니 실험도 제대로 작동했다.