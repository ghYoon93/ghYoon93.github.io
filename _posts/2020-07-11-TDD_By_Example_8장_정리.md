---
title: 객체 만들기
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
* **Dollar/Franc 중복**
* ~~공용 equals~~
* 공용 times
* ~~Franc과 Dollar 비교하기~~
* 통화?



```java
Dollar times(int multiplier) {
    return new Dollar(amount * multiplier);
}

Franc times(int multiplier) {
    return new Franc(amount * multiplier);
}
```

Dollar와 Franc의 times()가 거의 똑같다는 것을 한 눈에 알 수 있다.

반환 객체를 Money로 바꾸면 더 비슷하게 만들 수 있다.

```java
Money times(int multiplier) {
    return new Dollar(amount * multiplier);
}

Money times(int multiplier) {
    return new Franc(amount * multiplier);
}
```

이제 점점 Dollar와 Franc이 그다지 많은  일을 하지 않는 것 같으므로 필요가 없어진다.

하지만 이 두 클래스를 단번에 없애버리는 큰 단계는 TDD를 연습하기에는 적절하지 않다.

하위 클래스에 대한 직접적인 참조가 적어진다면 Money에 Dollar를 반환하는 **팩토리 메서드**를 도입할 수 있다.

```java
public void testMultiplication() {
    // Dollar five = new Dollar(5);
    Dollar five = Money.dollar(5);
    assertEquals(new Dollar(10), five.times(2));
    assertEquals(new Dollar(15), five.times(3));
}

static Dollar dollar(int amount) {
    return new Dollar(amount);
}
```

Dollar에 대한 참조를 더 지워보자

```java
public void testMultiplication() {
    Money five = Money.dollar(5);
    assertEquals(new Dollar(10), five.times(2));
    assertEquals(new Dollar(15), five.times(3));
}
```

Money 클래스에는 times()를 정의하지 않았으므로 컴파일 에러가 발생한다.

아직 times()를  어떻게 구현할지 생각하지 않았으므로 Money를 추상 클래스로 변경하고 Money.times()를 선언하자

```java
abstract class Money {
    abstract Money times(int multiplier);
}
```



팩토리 메서드의 선언도 Money를 반환하도록 바꿔주자.

```java
static Money dollar(int amount) {
    return new Dollar(amount);
}
```



팩토리 메서드를 변경했음에도 테스트는 정상적으로 작동한다. 이제 테스트 코드에 팩토리 메서드를 사용해보자

```java
public void testMultiplication() {
    Money five = Money.dollar(5);
    // assertEquals(new Dollar(10), five.times(2));
    assertEquals(Money.dollar(10), five.times(2));
    // assertEquals(new Dollar(15), five.times(3));
    assertEquals(Money.dollar(15), five.times(3));
}
    

public void testEquality() {
    // assertTrue(new Dollar(5).equals(new Dollar(5)));
    assertTrue(Money.dollar(5).equals(Money.dollar(5)));
    // assertFalse(new Dollar(5).equals(new Dollar(6)));
    assertFalse(Money.dollar(5).equals(Money.dollar(6)));
    assertTrue(new Franc(5).equals(new Franc(5)));
    assertFalse(new Franc(5).equals(new Franc(6)));
    // assertFalse(new Franc(5).equals(new Dollar(5)));
    assertFalse(new Franc(5).equals(Money.dollar(5)));
}
```

이제 Dollar 클래스가 있다는 사실을 어떤 클라이언트 코드도 알지 못한다.

하위 클래스의 존재를 테스트에서 분리(decoupling)함으로써 어떤 모델 코드에도 영향을 주지 않고 상속 구조를 마음대로 변경할 수 있게 됐다.



testFrancMultiplication도 위와 같은 작업을 하려고 보니 testMultiplication의 로직과 똑같다는 것을 발견했다.

그럼 이 테스트는 없어도 되는 것 같다. 하지만 코드에 대한 확신이 조금이라도 줄어들 가능성이 있을 수 있으므로 일단은 남겨두고 변경 작업을 진행하자.

```java
public void testEquality() {
    assertTrue(Money.dollar(5).equals(Money.dollar(5)));
    assertFalse(Money.dollar(5).equals(Money.dollar(6)));
    // assertTrue(new Franc(5).equals(new Franc(5)));
    assertTrue(Money.franc(5).equals(Money.franc(5)));
    // assertFalse(new Franc(5).equals(new Franc(6)));
    assertFalse(Money.franc(5).equals(new Franc(6)));
    // assertFalse(new Franc(5).equals(Money.dollar(5)));
    assertFalse(Money.franc(5).equals(Money.dollar(5)));
}
static Money franc(int amount) {
    return new Franc(amount);
}
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
* ~~5CHF X 2 = 10CHF~~
* Dollar/Franc 중복
* ~~공용 equals~~
* 공용 times
* ~~Franc과 Dollar 비교하기~~
* 통화?
* testFrancMultiplication을 지워야할까?



**검토**

* 동일한 메서드(times)의 두 변이형 메서드 서명부를 통일시킴으로써 중복 제거를 향해 한 단계 더 전진했다.
  * 메서드의 서명부: 메서드의 이름과 파라미터
* 최소한 메서드 선언부만이라도 공통 상위 클래스(superclass)로 옮겼다.(```abstract Money times(int multiplier)```)
* 팩토리 메서드를 도입하여 테스트 코드에서 콘크리트 하위 클래스의 존재 사실을 분리해냈다.
* 하위 클래스가 사라지면 몇몇 테스트는 불필요한 여분의 것이 된다는 것을 인식