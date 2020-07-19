---
title: 서로 다른 통화 더하기
categories: TDD
---



**통화가 다른 두 금액을 더해서 주어진 환율에 맞게 변한 금액을 결과로 얻을수 있어야한다.**

* **$5 + 10CHF = $10**
* ~~$5 + $5 = $10~~
* $5 + $5에서 Money 반환하기
* ~~Bank.reduce(Money)~~
* ~~Money에 대한 통화 변환을 수행하는 Reduce~~
* ~~Reduce(Bank, String)~~



```java
public void testMixedAddition() {
    Expression fiveBucks = Money.dollar(5);
    Expression tenFrancs = Money.franc(10);

    Bank bank = new Bank();
    bank.addRate("CHF", "USD", 2);

    Money result = bank.reduce(fiveBucks.plus(tenFrancs), "USD");

    assertEquals(Money.dollar(10), result);

}
```

이 테스트는 곧바로 컴파일할 수 없다.

처음 코드 수정이 다음 코드 수정으로 계속해서 퍼져나가게 진행할 예정이다.

```java
public void testMixedAddition() {
    // Expression fiveBucks = Money.dollar(5);
    // Expression tenFrancs = Money.franc(10);
    Expression fiveBucks = Money.dollar(5);
    Expression tenFrancs = Money.franc(10);

    Bank bank = new Bank();
    bank.addRate("CHF", "USD", 2);

    Money result = bank.reduce(fiveBucks.plus(tenFrancs), "USD");

    assertEquals(Money.dollar(10), result);

}
```



10달러를 예상했지만 15이 나왔다. Sum의 reduce()에서 인자를 축약하지 않았기 때문이다 (환율 반영 X).

```java
// Sum
public Money reduce(Bank bank, String to) {
    // int amount = augend.amount + addend.amount;
    int amount = augend.reduce(bank, to).amount 
        + addend.reduce(bank, to).amount;

    return new Money(amount, to);

}
```



테스트를 통과했다. Expression이어야 하는 Money들을 조금씩 쪼아서 없앨 수 있다. 가장자리부터 없애는 작업을 시작해 테스트 케이스까지 거슬러 올라가보자.

**augend와 addend를 Expression으로 취급**

```java
// Sum
// int augend;
// int addend;
Expression augend;
Expression addend;
```

**Sum 생성자의 인자를 Expression으로 취급**

```java
Sum(Expression augend, Expression addend) {
    this.augend = augend;
    this.addend = addend;
}
```



**Money에서 plus()와 times()의 반환 값을 Expression으로 취급**

```java
Expression plus(Expression addend) {
    return new Sum(this, addend);
}

Expression times(int multiplier) {
    return new Money(amount * multiplier, currency);
};
```

이코는 Expression이 plus()와 times() 오퍼레이션을 포함해야 함을 제안하고 있다.

이게 Money의 전부이다. 



**테스트 케이스의 plus()의 인자 바꾸기**

```java
// MoneyTest
public void testMixedAddition() {
    Expression tenFransc = Money.franc(10);
}
```



**fivebucks의 타입을 Expression으로 바꾸기**

```java
// MoneyTest
public void testMixedAddition() {
    // Money fivebucks = Money.dollar(5);
    Expression fivebucks = Money.franc(10);
}
```



**Expression에 plus() 정의하기**

```java
// Expression
Expression plus(Expression addend);
```



Expression에 plus()를 정의했더니 두개의 컴파일 에러가 발생했다. 

1. cannot reduce the visibility of the inherited method from Expression
2. The type Sum must implement the inherited abstract method Expression.plus(Expression)

```java
// Money
public Expression plus(Expression addend) {
    return new Sum(this, addend);
}

// Sum
public Expression plus(Expression addend) {
    return null;
}
```

첫번째 에러는 접근 범위를 공용으로 바꿔줌으로 해결했고 두번째 에러는 일단 오버라이드함으로 에러를 해결했고 구현은 추후에 할 예정이다.



**통화가 다른 두 금액을 더해서 주어진 환율에 맞게 변한 금액을 결과로 얻을수 있어야한다.**

* ~~$5 + 10CHF = $10~~
* ~~$5 + $5 = $10~~
* $5 + $5에서 Money 반환하기
* ~~Bank.reduce(Money)~~
* ~~Money에 대한 통화 변환을 수행하는 Reduce~~
* ~~Reduce(Bank, String)~~
* Sum.plus
* Expression.times



**검토**

* 원하는 테스트를 작성하고, 한 단계에 달성할 수 있도록 뒤로 물렀다. (Expression -> Money)
* 좀 더 추상적인 선언을 통해 가지에서 뿌리(애초의 테스트 케이스)로 일반화했다. (오퍼레이션의 코드의 Money -> Expression)
* 변경 후 (Expression fiveBucks), 그 영향을 받은 다른 부분들을 변경하기 위해 컴파일러의 지시를 따랐다.(Expression에 plus() 추가하기 등등)