---
title: 사과와 오렌지
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
* 공용 times
* **Franc과 Dollar 비교하기**



```java
public void testEquality() {
    assertTrue(new Franc(5).equals(new Dollar(5)));
}
```

위 코드는 AssertionError를 발생시킨다. 

두 객체의 클래스를 비교하는 것을 추가하여 테스트를 통과해보자.

두 Money는 오직 금액과 클래스가 서로 동일할 때만 같다.

```java
public boolean equals(Object object) {
    Money money = (Money) object;
    return amount == money.amount && getClass().equals(money.getClass());
}
```

현실 세계의 재정 분야에 맞는 용어를 사용하고 싶지만 통화 개념도 없고 도입할만한 이유가 없으므로 자바 객체 용어를 사용한다.

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
* 공용 times
* ~~Franc과 Dollar 비교하기~~
* 통화

이제 공용times()를 처리할 때다. 그 이전에 혼합된 통화 간의 연산에 대해 다뤄야한다.

**검토**

* 묵혀뒀던 결함을 꺼내 테스트에 담아냈다. (```new Franc(5).equals(new Dollar(5));```)
* 그럭저럭 봐줄만한 방법(```get class();```)로 테스트를 통과했다.
* 더 많은 동기가 있기 전에는 더 많은 설계를 도입하지 않았다. (재정 분야 용어 대신 자바 객체 용어 사용)