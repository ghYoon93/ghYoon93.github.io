---
title: 드디어, 더하기
categories: TDD
---



**통화가 다른 두 금액을 더해서 주어진 환율에 맞게 변한 금액을 결과로 얻을수 있어야한다.**

* $5 + 10CHF = $10
* **$5 + $5 = $10**



```java
// MoneyTest
public void testSimpleAddition() {
    Money sum = Money.dollar(5).plus(Money.dollar(5));
    assertEquals(Money.dollar(10), sum);
}

// Money
Money plus(Money addend) {
    return new Money(amount + addend.amount, currency);
}
```

설계상 가장 어려운 제약은 다중 통화 사용에 대한 내용을 시스템의 나머지 코드에게 숨기고 싶다는 점이다.

한가지 가능한 전략은 모든 내부값을 참조통화(복합 채권이나 환전 등에서 기준이 되는 화폐)로 전환하는 것이다.

하지만 이 방식으로는 여러 환율을 쓰기가 쉽지 한다.

대신, 편하게 여러 환율을 표현할 수 있으면서도 산술 연산 비슷한 표현들을 여전히 산술 연산처럼 다룰 수 있는 해법이 있으면 좋을 것 같다.

객체가 우리를구해줄 것이다. 가지고 있는 객체와 외부 프로토콜이 같으면서 내부 구현은 다른 새로운 객체(imposter)를 만들 수 있다.

해법은 Money와 비슷하게 동작하지만 사실은 두 Money의 합을 나타내는 객체를 만드는 것이다.



**Expression: Money의 합을 나타내는 객체**

```java
// Expression
interface Expression(){}

// Money
Expression plus(Money addend) {
    return new Money(amount + addend.amount, currency);
}
// Money
class Money implements Expression(){}
    
// Bank
class Bank {
    Money reduce(Expressino source, String to) {
        return Money.dollar(10);
    }
}
```



**검토**

* 큰 테스트를 작은 테스트 ($5 + 10CHF -> $5 + $5)로 줄여서 발전을 나타낼 수 있도록 했다.
* 우리에게 필요한 계산(computation)에 대한 가능한 메타포들을 신중히 생각해봤다.
* 새 메타포에 기반하여 기존의 테스트를 재작성
* 테스트를 빠르게 컴파일
* 테스트를 실행