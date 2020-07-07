---
title: 다중 통화를 지원하는 Money 객체
categories: TIL
tags: TDD
---



**TDD의 리듬**

1. 재빨리 테스트를 하나 추가한다.
2. 모든 테스트를 실행하고 새로 추가한 것이 실패하는지 확인한다.
3. 코드를 조금 바꾼다.
4. 모든 테스트를 실행하고 전부 성공하는지 확인한다.
5. 리팩토링을 통해 중복을 제거한다.



* 각각의 테스트가 **기능의 작은 증가분을 어떻게 커버**하는지
* 새 테스트를 돌아가게 하기 위해 **얼마나 작고 못생긴 변화가 가능한지**
* 얼마나 **자주 테스트를 실행**하는지
* 얼마나 **수 없이 작은 단계**를 통해 리팩토링이 되어가는지



**할 일 목록**

* 통화가 다른 두 금액을 더해서 주어진 환율에 맞게 변한 금액을 결과로 얻을 수 있어야한다.

* 어떤 금액(주가)을 어떤 주(주식의 수)에 곱한 금액을 결과로 얻을 수 있어야 한다.

  

테스트를 작성할 때는 오퍼레이션의 완변한 인터페이스에 대해 상상해보는 것이 좋다.

```오퍼레이션: 객체가 수행할 수 있는 연산, 메서드와 비슷한 의미로 쓰이며 엄격하게는 오퍼레이션에 대한 특정한 하나의 구현을 메서드라 한다.```



**어떤 금액(주가)을 어떤 주(주식의 수)에 곱한 금액을 결과로 얻을 수 있어야 한다.**

* $5 + 10CHF = $10
* **$5 X 2 = $10**
* amount를 private으로 만들기
* Dollar 부작용(side effect)?
* Dollar 반올림?



재빨리 테스트를 하나 추가한다.

모든 테스트를 실행하고 새로 추가한 것이 실패하는지 확인한다.

```java
public class MoneyTest {

    @Test
    public void testMultiplication() {
        Dollar five = new Dollar(5);
        five.times(2);
        assertEquals(10, five.amount);
    }

```

**컴파일 에러 발생**

* Dollar 클래스가 없다.
* 생성자가 없다.
* times(int) 메서드가 없다.
* amount 필드가 없다.



**Dollar 클래스가 없다**

```java
public class Dollar {}
```

**생성자가 없다**

```java
public Dollar(int amount) {}
```

**times(int) 메서드가 없다.**

```java
void times(int multiplier) {}
```

**amount 필드가 없다.**

```java
int amount;
```



코드를 조금 바꾼다.

모든 테스트를 실행하고 전부 성공하는지 확인한다.

```java
int amount = 10;
```



중복을 제거한다

```java
// int amount = 10;
// int amount = 5 * 2;
int amount;

void times(int multiplier) {
    amount = 5 * 2;
}
```

테스트 코드와 작업 코드 사이의 중복 (5를 어디서 얻을 수 있을까?) -> 생성자를 통해서

```java
Dollar(int amount) {
    this.amount = amount;
}
//void times(int multiplier) {
//    amount = amount * 2;
//}
void times(int multiplier) {
    amount *= multiplier;
}
```



**어떤 금액(주가)을 어떤 주(주식의 수)에 곱한 금액을 결과로 얻을 수 있어야 한다.**

* $5 + 10CHF = $10
* ~~$5 X 2 = $10~~
* amount를 private으로 만들기
* Dollar 부작용(side effect)?
* Dollar 반올림?

```java
public class Dollar {
    int amount;
    public Dollar(int amount) {
        this.amount = amount;
    }
    void times(int multiplier) {
        amount *= multiplier;
    }
}
```



