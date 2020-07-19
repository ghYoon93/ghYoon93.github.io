---
title: 드디어, 추상화
categories: TDD
---



**통화가 다른 두 금액을 더해서 주어진 환율에 맞게 변한 금액을 결과로 얻을수 있어야한다.**

* ~~$5 + 10CHF = $10~~
* ~~$5 + $5 = $10~~
* $5 + $5에서 Money 반환하기
* ~~Bank.reduce(Money)~~
* ~~Money에 대한 통화 변환을 수행하는 Reduce~~
* ~~Reduce(Bank, String)~~
* Sum.plus
* Expression.times



**Sum.plus()에 대한 테스트**

```java
public void testSumPlusMoney() {
    Expression fiveBucks = Money.dollar(5);
    Expression tenFrancs = Money.franc(10);

    Bank bank = new Bank();
    bank.addRate("CHF", "USD", 2);

    Expression sum = new Sum(fiveBucks, tenFrancs).plus(fiveBucks);
    Money result = bank.reduce(sum, "USD");

    assertEquals(Money.dollar(15), result);
}
```

fiveBucks와 tenFrancs를 더해 Sum을 생성할 수도 있지만 명시적으로 Sum을 생성했다. 이 편이 더 직접적으로 의도를 드러낼 수 있기 때문이다.



테스트가 코드보다 길다. 그리고 코드는 Money의 코드와 똑같다.(추상화를 할 수 있는 조건에 만족한다.)

```java
// Sum
public Expression plus(Expression addend) {
    // return null;
    return new Sum(this, addend);
}
```



TDD로 구현할 때에는 테스트 코드의 줄 수와 모델 코드의 줄 수가 거의 비슷한 상태로 끝난다.

TDD가 경제적이기 위해서는 매일 만들어 내는 코드의 줄 수가 두 배가 되거나 동일한 기능을 구현하되 절반의 줄 수로 해내야 할것이다.

TDD가 자신의 방법에 비해 어떻게 다른지 직접 측정해보자. 이때 디버깅, 통합 작업, 다른 사람에게 설명하는 데 걸리는 시간 등의 요소를 반드시 포함하자.



**통화가 다른 두 금액을 더해서 주어진 환율에 맞게 변한 금액을 결과로 얻을수 있어야한다.**

* ~~$5 + 10CHF = $10~~
* ~~$5 + $5 = $10~~
* $5 + $5에서 Money 반환하기
* ~~Bank.reduce(Money)~~
* ~~Money에 대한 통화 변환을 수행하는 Reduce~~
* ~~Reduce(Bank, String)~~
* ~~Sum.plus~~
* **Expression.times**



일단 Sum.times()가 작동하게 만들 수만 있다면 Expression.times()를 선언하는 일이야 어렵지 않다.

**Sum.times() 테스트**

```java
public void testSumTimes() {
    Expression fiveBucks = Money.dollar(5);
    Expression tenFrancs = Money.franc(10);

    Bank bank = new Bank();
    bank.addRate("CHF", "USD", 2);

    Expression sum = new Sum(fiveBucks, tenFrancs).times(2);
    Money result = bank.reduce(sum, "USD");

    assertEquals(Money.dollar(20), result);
}
```

테스트 코드가 길다.(29장 픽스처(Fixture) 참고)



```java
// Sum
Expression times(int multiplier) {
    return new Sum(augend.times(multiplier), addend.times(multiplier));
}
```

augend와 addend를 Expression으로 추상화했기 때문에 Expression에 times()를 선언해야한다.

```java
// Expression
Expression times(int multiplier);
```



**$5 + $5에서 Money 반환**

```java
public void testPlusSameCurrencyReturnsMoney() {
    Expression sum = Money.dollar(1).plus(Money.dollar(1));
    assertTrue(sum instanceof Money);
}
```

이 테스트는 외부에 드러나는 객체의 행위에 대한 테스트가 아니라 구현 중심의 테스트이기 때문에 좀 지저분하다.

하지만 이 테스트는 원하는 변화를 가할 수 있게 해준다. 이 테스트를 통과시키기 위해 다음과 같이 쓸 수 있다.

```java
// Money
public Expression plus(Expression, addend) {
    return new Sum(this, addend);
}
```

인자가 Money일 경우에, 오로지 그 경우에만(필요충분조건으로) 인자의 통화를 확인하는 분명하고 깔끔한 방법이 없다. 실험은 실패했고, 우리는 테스트를 삭제하고 떠난다.



**검토**

* 미래에 코드를 읽을 다른 사람들을 염두에 둔 테스트를 작성했다.
* 선언부에 대한 수정이 시스템 나머지 부분으로 번져갔고, 문제를 고치기 위해 컴파일러의 조언을 따랐다.
* 잠시 테스트를 시도했는데 제대로 되지 않아서 버렸다.