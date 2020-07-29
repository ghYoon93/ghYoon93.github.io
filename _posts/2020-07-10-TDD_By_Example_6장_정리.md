---
title: 돌아온 '모두를 위한 평등'
categires: TIL
tags: TDD
---

**어떤 금액(주가)을 어떤 주(주식의 수)에 곱한 금액을 결과로 얻을 수 있어야 한다.**

* ~~$5 X 2 = $10 (1)~~
* ~~amount를 private으로 만들기 (4)~~
* ~~Dollar 부작용(side effect) (2)?~~
* Money 반올림?
* ~~equals() (3)~~
* hashCode()
* Equal null
* Equal object
* ~~5CHF X 2 = 10CHF (5)~~
* Dollar/Franc 중복
* **공용 equals (6)**
* 공용 times



이제 중복을 청소해보자. 가능한 방법 하나는 두 클래스의 공통 상위 클래스를 찾는 것이다.

Dollar와 Franc의 공통점은 무엇일까? 바로 돈의 단위라는 것이다.

Money 클래스를 만들어보자.

```java
class  Money
```

기존 테스트에는 아무 이상이 없다.

Dollar 클래스에서 Money 클래스를 상속받아보자.

```java
class Dollar exnteds Money {
    private int amount;
}
```

여전히 테스트는 이상없이 잘 돌아간다. 이제 amount 인스턴스 변수를 Money로 옮기자.

```java
class Money {
    protected int amount;
}
```

하위 클래스에서 변수를 볼 수 있도록 접근자를 ```protected```로 지정했다.



이제 ```equals()``` 를 상위 클래스로 올리는 일을 해보자

우선 임시 변수(dollar)를 선언하는 부분을 변경하자

```java
public boolean equals(Object object) {
    Money money = (Money) object;
    return amount == money.amount;
}
```

```testEquality()```가 Franc끼리의 비교에 대해서는 다루지 않는다.

```Franc.equals()```를 제거하기 전에 그 곳에 원래 있어야했던 테스트를 작성하자

적절한 테스트를 갖지 못한 코드에서 TDD를 해야하는 경우가 종종 있을 것이다.

충분한 테스트가 없다면 지원 테스트가 갖춰지지 않은 리팩토링을 만나게 될 수 밖에 없다.

리팩토링하면서 실수했는데도 불구하고 테스트가 여전히 통과할 수도 있는 것이다.

어떻게 해야할까?
있으면 좋을 것 같은 테스트를 작성하라. 
그렇게 하지 않으면 결국 어느 시점에서 리팩토링하다가 무언가를 깨트릴 것이다.
 이는 리팩토링에 대한 안 좋은 기억을 갖게 하고 성장에 도움이 되지 않는다.



```java
public void testEquality() {
    assertTrue(new Dollar(5).equals(new Dollar(5)));
    assertFalse(new Dollar(5).equals(new Dollar(6)));
    assertTrue(new Franc(5).equals(new Franc(5)));
    assertFalse(new Franc(5).equals(new Franc(6)));
}
```

중복이 발생하지만 Franc에 대한 동치성 테스트를 작성했다.

Franc 클래스도 Dollar 클래스와 마찬가지로 Money를 상속받고 중복을 제거해보자

```java
public class Franc extends Money{
    
    public Franc(int amount) {
        this.amount = amount;
    }
    
    Franc times(int multiplier) {
        return new Franc(amount * multiplier);
    }
}
```



**어떤 금액(주가)을 어떤 주(주식의 수)에 곱한 금액을 결과로 얻을 수 있어야 한다.**

* ~~$5 X 2 = $10 (1)~~
* ~~amount를 private으로 만들기 (4)~~
* ~~Dollar 부작용(side effect) (2)?~~
* Money 반올림?
* ~~equals() (3)~~
* hashCode()
* Equal null
* Equal object
* ~~5CHF X 2 = 10CHF (5)~~
* Dollar/Franc 중복
* ~~공용 equals (6)~~
* 공용 times



**검토**

* 공통된 코드를 첫 번째 클래스(Dollar)에서 상위 클래스(Money)로 단계적으로 옮겼다.
  * 상위 클래스를 만든다 (```class Money```)
  * 하위 클래스에서 상위 클래스를 상속받는다. (```Dollar extends Money```)
  * 인스턴스 변수를 상위 클래스로 옮긴다. (```int amount```)
  * 공용 메서드를 상위 클래스에 맞게 고친 후 옮긴다. (```equals(Object o)```)
* 두번째 클래스(Franc)도 Money의 하위 클래스로 만들었다.
* 불필요한 구현을 제거하기 전에 두 equals() 구현을 일치시켰다.