---
title: 타락한 객체
categories: TIL
tags: TDD
---

>times 연산 후 five는 더이상 5의 값을 갖지 않는다.
>
>five는 언제나 5가 될 수 있도록 리팩토링 해보자



**어떤 금액(주가)을 어떤 주(주식의 수)에 곱한 금액을 결과로 얻을 수 있어야 한다.**

* $5 + 10CHF = $10
* ~~$5 X 2 = $10~~
* amount를 private으로 만들기
* **Dollar 부작용(side effect)?**
* Money 반올림?



**Dollar 부작용?**

Dollar에 대한 연산을 수행한 후에 해당 Dollar의 값이 바뀐다. 

```java
public void testMultiplication() {
    Dollar five = new Dollar(5);
    five.times(2);
    assertEquals(10, five.amount);
    five.times(3);
    assertEquals(15, five.amount);
}
```

times()에서 새로운 객체를 반환하게 만들자

```java
public void testMultiplication() {
        Dollar five = new Dollar(5);
        five.times(2);
        Dollar product = five.times(2);
        assertEquals(10, product.amount);
        product = five.times(3);
        assertEquals(15, product.amount);
}
Dollar times(int multiplier) {
    // amount *= multiplier;
    // return null;
    return new Dollar(amount * multiplier);
}
```



**어떤 금액(주가)을 어떤 주(주식의 수)에 곱한 금액을 결과로 얻을 수 있어야 한다.**

* $5 + 10CHF = $10
* ~~$5 X 2 = $10~~
* amount를 private으로 만들기
* ~~Dollar 부작용(side effect)?~~
* Money 반올림?

```java
public class Dollar {
    int amount;
    public Dollar(int amount) {
        this.amount = amount;
    }
    Dollar times(int multiplier) {
        return new Dollar(amount * multiplier);
    }
}
```



초록 막대를 최대한 빨리 보기 위한 전략

* 가짜로 구현하기: 상수를 반환하게 만들고 진짜 코드를 얻을 때까지 단계적으로 상수를 변수로 바꾸어 간다.
* 명백한 구현 사용하기: 실제 구현을 입력한다.
* 삼각측량



**검토**

* 설계상 결함(Dollar 부작용)을 그 결함으로 인해 실패하는 테스트로 변환
* 스텁 구현으로 빠르게 컴파일을 통과
* 올발다고 생각하는 코드를 입력하여 테스트를 통과