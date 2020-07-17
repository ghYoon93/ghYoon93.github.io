---
title: 진짜로 만들기
categories: TDD
---



**통화가 다른 두 금액을 더해서 주어진 환율에 맞게 변한 금액을 결과로 얻을수 있어야한다.**

* $5 + 10CHF = $10
* **$5 + $5 = $10**



**데이터중복**

```java
// Bank
Money reduce(Expression source, String to) {
    return Money.dollar(10);
}

// MoneyTest
public void testSimpleAddition() {
    Money five = Money.dollar(5);
    Expression sum = five.plus(five);
    Bank bank = new Bank();
    Money reduced = bank.reduce(sum, "USD");
    assertEquals(Money.dollar(10), redocued);
}
```

```Money.dollar(10);``` 과 ```five.plus(five);```는 사실 상 같다.



**통화가 다른 두 금액을 더해서 주어진 환율에 맞게 변한 금액을 결과로 얻을수 있어야한다.**

* $5 + 10CHF = $10
* **$5 + $5 = $10**
* $5 + $5에서 Money 반환하기

Money.plus()는 이제 Money가 아닌 Expression(Sum)을 반환해야한다. 두 Money의 합은 Sum이어야한다.

```java
// MoneyTest
public void testReduceSum() {
    Expression sum = new Sum(Money.dollar(3), Money.dollar(4));
    Bank bank = new Bank();
    Money result = bank.reduce(sum, "USD");
    assertEqulas(Money.dollar(7), result);
}

// Bank
Money reduce(Expession source, String to) {
    Sum sum = (Sum) source;
    int amount = sum.augend.amount + sum.addend.amount;
    return new Money(amount, to);
}
```

* 캐스팅(형변환). 이 코드는 모든 Expression에 대해 작동해야한다.
* 공용(public)필드와 그 필드들에 대한 두 단계에 걸친 레퍼런스.



**통화가 다른 두 금액을 더해서 주어진 환율에 맞게 변한 금액을 결과로 얻을수 있어야한다.**

* $5 + 10CHF = $10
* **$5 + $5 = $10**
* $5 + $5에서 Money 반환하기
* Bank.reduce(Money)



**클래스를 명시적으로 검사하는 코드가 있을 때에는 항상 다형성을 사용하도록 바꾸는 것이 좋다.**

Sum은 reduce(String)을 구현하므로, Money도 그것을 구현하도록 만든다면 reduce()를 Expression 인터페이스에도 추가할 수 있게 된다.

```java
// Money
public Money reduce(String to) {
    return this;
}

// Bank
Money reduce(Expression source, String to) {
    // if(source instanceof Money) return (Money) source.reduce(to);
    // Sum sum = (Sum) source;
    // return sum.reduce(to);
    return source.reduce(to);
}

// Expression
Money reduce(String to);

```

**통화가 다른 두 금액을 더해서 주어진 환율에 맞게 변한 금액을 결과로 얻을수 있어야한다.**

* $5 + 10CHF = $10
* **$5 + $5 = $10**
* $5 + $5에서 Money 반환하기
* ~~Bank.reduce(Money)~~
* Money에 대한 통화 변환을 수행하는 Reduce
* Reduce(Bank, String)



**검토**

* 모든 중복이 제거되기 전까지는 테스트를 통과한 것으로 치지 않았다.
* 구현하기 위해 역방향이 아닌 순방향으로 작업했다.
* 앞으로 필요할 것으로 예상되는 객체(Sum)의 생성을 강요하기 위한 테스트 작성
* 빠른 속도로 구현하기 시작(Sum의 생성자)
* 일단 한 곳에 캐스팅을 이용해서 코드를 구현했다가, 테스트가 돌아가자 그 코드를 적당한 자리로 옮김
* 명시적인 클래스 검사를 제거하기 위해 다형성 사용