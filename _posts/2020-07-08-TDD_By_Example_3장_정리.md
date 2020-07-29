---
title: 모두를 위한 평등
categories: TIL
tags: TDD
---



### Value Obejct Pattern

Dollar 객체같이 객체를 값처럼 쓸 수 있는 것을 **값 객체 패턴 (value object pattern)**이라 한다.
객체의 인스턴수 변수가 생성자를 통해서 설정된 후에는 결코 변하지 않는다.



**새로운 할 일 목록 추가**

값 객체가 암시하는 것 중 하나는 모든 연산은 새 객체를 반환해야한다는 것, 다른 하나는 값 객체는 equals()를 구현해야한다는 것.

Dollar를 해시 테이블의 키로 쓸 생각이라면 equals()를 구현할 때 hashCode()를 같이 구현해야한다.



**어떤 금액(주가)을 어떤 주(주식의 수)에 곱한 금액을 결과로 얻을 수 있어야 한다.**

* ~~$5 X 2 = $10 (1)~~
* amount를 private으로 만들기
* ~~Dollar 부작용(side effect)? (2)~~
* Money 반올림?
* **equals() (3)**
* hashCode()

```java
public void testEquality() {
    assertTrue(new Dollar(5).equals(new Dollar(5)));
}

public boolean equals(Object object) {
    return true;
}
```

true라는 것은 '5 == 5'라는 것이고 이는 'amount == 5'이며 결국 'amount == dollar.amount'임을 뜻한다.



**삼각 측량**

**라디오 신호**를 **두 수신국**이 감지하고 있을 때, **수신국 사이의 거리**가 알려져 있고 각 수신국이 **신호의 방향**을 알고 있다면, 이 정보만드로 충분히 **신호의 거리와 방위**를 알 수 있다.

삼각측량을 이용하려면 예제가 두 개 이상 있어야만 코드를 일반화할 수 있다.
두 번째 예가 좀 더 일반적인 해를 필요로 할 때, 그때 비로소 일반화 한다.

**두번째 예**

```java
assertFalse(new Dollar(5).equals(new Dollar(6)));
```

**동치성 일반화**

```java
public boolean equals(Object object) {
    Dollar dollar = (Dollar) object;
    return amount == dollar.amount;
}
```



**어떤 금액(주가)을 어떤 주(주식의 수)에 곱한 금액을 결과로 얻을 수 있어야 한다.**

* ~~$5 X 2 = $10 (1)~~
* amount를 private으로 만들기
* ~~Dollar 부작용(side effect)? (2)~~
* Money 반올림?
* ~~equals() (3)~~
* hashCode()
* Equal null
* Equal object



어떻게 리팩토링을 해야 하는지 감이 오지않을 때 삼각측량을 사용하면 좋다.

설계를 어떻게 해야할지 떠오르지 않을  때 삼각측량은 문제를 다른 방향에서 생각해볼 기회를 제공한다.

지금 설계하는 프로그램이 **어떤 변화 가능성을 지원해야하는가?**



**검토**

* 디자인 패턴(값 객체)이 하나의 또 다른 오퍼레이션을 암시한다
* 해당 오퍼레이션을 테스트 ```assertTrue(a.equals(b))```
* 해당 오퍼레이션을 간단히 구현 ```public boolean equals(O o)```
* 곧장 리팩토링하는 대신 테스트를 조금 더 했다. ```assertFalse(a.equals(b));```
* 두 경우를 모두 수용할 수 있도록 리팩토링했다. ```return amount == dollar.amount```