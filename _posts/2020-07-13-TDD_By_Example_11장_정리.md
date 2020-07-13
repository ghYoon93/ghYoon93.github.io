---
title: 모든 악의 근원
categories: TDD
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
* **Dollar/Franc 중복**
* ~~공용 equals~~
* ~~공용 times~~
* ~~Franc과 Dollar 비교하기~~
* ~~통화?~~
* testFrancMultiplication을 지워야할까?



Dollar와 Franc에는 생성자밖에 없다. 생성자 때문에 하위 클래스가 있을 필요가 없으므로 Dollar와 Franc을 제거해보자

코드의 의미를 변경하지 않으면서도 하위 클래스에 대한 참조를 상위 클래스에 대한 참조로 변경할 수 있다.

```java
// Money
static Money dollar(int amount) {
    // return new Dollar(amount, "USD");
    return new Money(amount, "USD");
}

static Money franc(int amount) {
    // return new Franc(amount, "CHF");
    return new Money(amount, "CHF");
}
```

이제 Dollar를 참조하는 것은 하나도 남아 있지 않으므로 Dollar를 지울 수 있다. 하지만 Franc은 MoneyTest의 testDiffrentClassEquality()에서 참조하고 있다.

testEquality()가 동치성 테스트를 충분히 하고 있으므로 testDiffrentClassEquality()는 없어도 될 것 같다.

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
* ~~Dollar/Franc 중복~~
* ~~공용 equals~~
* ~~공용 times~~
* ~~Franc과 Dollar 비교하기~~
* ~~통화?~~
* ~~testFrancMultiplication을 지워야할까?~~

클래스 대신 currency를 비교하도록 강요하는 테스트 코드는 여러 클래스가 존재할 때만 의미가 있다.

**검토**

* 하위 클래스의 속을 들어내는 것을 완료하고, 하위 클래스를 삭제했다.
* 기존 소스 구조에서는 필요했지만 새로운 구조에서는 필요 없게 된 테스트를 제거했다.