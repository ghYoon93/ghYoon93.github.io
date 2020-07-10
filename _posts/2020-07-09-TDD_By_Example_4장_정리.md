---
title: 프라이버시
categories: TIL
tags: TDD
---

**어떤 금액(주가)을 어떤 주(주식의 수)에 곱한 금액을 결과로 얻을 수 있어야 한다.**

* $5 + 10CHF = $10
* ~~$5 X 2 = $10~~
* **amount를 private으로 만들기**
* ~~Dollar 부작용(side effect)?~~
* Money 반올림?
* ~~equals()~~
* hashCode()
* Equal null
* Equal object



```Dollar.times()``` 연산은 호출을 받은 객체의 값에 인자로 받은 곱수만큼 곱한 값을 갖는 Dollar를 반환해야한다.
하지만 지금 테스트 클래스에서는 명확히 그것을 말하지 않는다.

```java
public void testMultiplication() {
    Dollar five = new Dollar(5);
    Dollar product = five.times(2);
    // assertEquals(10, product.amount);
    assertEquals(new Dollar(10), product);
    product = five.times(3);
    // assertEquals(15, product.amount);
    assertEquals(new Dollar(15), product);
}
```

```assertEquals(10, product.amount)``` 단언을 ```assertEquals(new Dollar(10), product)```로 바꿈으로 ````Dollar.times()``` 연산이 amount가 10인 Dollar 객체를 반환한다는 것을 명확히할 수 있다.

이제 amount를 확인하기위해 선언했던 product 변수는 필요가 없으므로 product 변수를 없애고 단언을 다음과 같이 바꿔준다.

```java
public void testMultiplication() {
    Dollar five = new Dollar(5);
    // Dollar product = five.times(2);
    assertEquals(new Dollar(10), five.times(2));
    // product = five.times(3);
    assertEquals(new Dollar(15), five.times(3));
}
```

이제 Dollar의 amount 인스턴수 변수를 사용하는 코드는 자기 자신밖에 없게 되므로 amount를 private으로 바꿔줄 수 있다.

```java
private int amount;
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



**검토**

* 오직 테스트를 향상시키기 위해서만 개발된 기능을 사용 (```assertEquals(new Dollar(10), product)```)
* 두 테스트가 동시에 실패하면 망한다. (equality 테스트가 실패하면 multiplication 테스트도 실패)
* 위험 요소가 있음에도 계속 진행 
* 테스트와 코드 사이의 결합도를 낮추기위해, 테스트하는 객체의 새 기능을 사용 (테스트에서 amount 제거)