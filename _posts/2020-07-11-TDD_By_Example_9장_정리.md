---
title: 우리가 사는 시간
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
* 공용 times
* ~~Franc과 Dollar 비교하기 (7)~~ 
* **통화 (9)**
* testFrancMultiplication을 지워야할까?



불필요한 하위 클래스를 제거하기 전에 통화 개념을 도입해보자.

통화 개념을 어떻게 구현해야할까 생각하기 먼저 통화 개념을 어떻게 테스트할까부터 생각하라.

통화를 표현하기 위한 복잡한 객체들을 원할 수도 있다. 그리고 경량 팩토리를 사용해서 그 객체들이 필요한 만큼만 만들어지도록 할 수 있다. 하지만 당분간은 경량 팩토리 대신 문자열을 쓰자.

```java
public void testCurrency() {
    assertEquals("USD", Money.dollar(1).currency());
    assertEquals("CHF", Money.franc(1).currency());
}
// Money
abstract String currency;

// Franc
String currency() {
    return "CHF";
}

// Dollar
String currency() {
    return "USD";
}
```



우린 두 클래스를 모두 포함할 수 있는 동일한 구현을 원한다.

통화를 인스턴스 변수에 저장하고, 메서드에서는 그냥 그걸 반환하게 만들어보자.

```java
// Franc
private String currency;
Franc(int amount) {
    this.amount = amount;
    currency = "CHF";
}
String currency() {
    // return "CHF";
    return currency;
}

// Dollar
private String currency;
Dollar(int amount) {
    this.amount = amount;
    currency = "USD";
}
String currency() {
    // return "USD";
    return currency();
}
```

이제 두 currency()가 동일하므로 변수 선언과 currency() 구현을 위로 올릴 수 있게 됐다.

```java
// Money
protected String currency;
String currency() {
    return currency;
}
```

문자열 'USD'와 'CHF'를 정적 팩토리 메서드로 옮긴다면 두 생성자가 동일해질 것이고, 공통 구현을 만들 수 있을 것이다.

```java
// Franc
Franc(int amount, String currency) {
    this.amount = amount;
    // currency = "CHF";
    this.currency = "CHF";
}

// Money
static Money franc(int amount) {
    return new Franc(amount, null);
}

// Franc
Money times(int multiplier) {
    return new Franc(amount * multiplier, null);
}
```

이 때 우리는 이전에 생성했던 팩토리 메서드를 호출하지 않고 생성자를 호출하는 것인지 의문이 든다.

지금 이걸 팩토리 메서드로 고쳐야할까 아니면 통화 개념을 도입될 때까지 기다려야할까 고민이 된다.

짧은 중단일 때는 지금 하는 일을 잠시 멈추는 것도 괜찮다. 하지만 이 중단에서 또 다른 중단이 발생할 때는 그 일을 또 중단하지는 않는다.

```java
// Franc
Money times(int multiplier) {
    // return new Franc(amount * multipiler, null);
    return Money.franc(amount * multiplier);
}

// Money
static Money franc(int amount) {
    // return new Franc(amount, null);
    return new Franc(amount, "CHF");
}

// Franc
Franc(int amount, String currency) {
    this.amount = amount;
    // this.currency = "CHF";
    this.currency = currency;
}
```

Dollar도 한번 바꿔보자

```java
// Money
static Money dollar(int amount) {
    // reutrn new Dollar(amount);
    return new Dollar(amount, "USD");
}

// Dollar
Dollar(int amount, String currency) {
    this.amount = amount;
    // currency = "USD";
    this.currency = currency;
}
Money times(int multiplier) {
    // return new Dollar(amount * multiplier);
    return Money.dollar(amount * multiplier);
}
```

테스트는 이상 없이 작동된다. 이제 Dollar와 Franc의 생성자가 동일해졌다. 생성자 구현을 상위 클래스에 올리자.

```java
// Money
Money(int amount, String currency) {
    this.amount = amount;
    this.currency = currency;
}

// Franc
Franc(int amount, String currency) {
    // this.amount = amount;
    // this. currency = currency;
    super(amount, currency);
}

// Dollar
Dollar(int amount, String currency) {
    // this.amount = amount;
    // this.currency = currency;
    super(amount, currency);
}

```



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
* 공용 times
* ~~Franc과 Dollar 비교하기 (7)~~ 
* ~~통화 (9)~~
* testFrancMultiplication을 지워야할까?





times()를 상위 클래스에 올리고 하위 클래스들을 제거할 준비가 거의 다 됐다.



**검토**

* 큰 설계 아이디어를 다루다가 조금 곤경에 빠졌다. 그래서 좀 전에 주목했던 더 작은 작업을 수행했다.
* 다른 부분들을 호출자(팩토리 메서드)로 옮김으로써 두 생성자를 일치시켰다.
* times()가 팩토리메서드를사용하도록 만들기 위해 리팩토링을 잠시 중단했다.
* 비슷한 리팩토링(Franc에 했던 일을 Dollar에도 적용)을 한번의 큰 단계로 처리했다.
* 동일한 생성자들을 상위 클래스로 올렸다.