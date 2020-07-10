---
title: 솔직히 말하자면
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
* **5CHF X 2 = 10CHF**



```$5  + 10CHF = $10``` 을 테스트하기 전에 Franc에 대한 테스트를 먼저 진행해보자.

```java
public void testFrancMultiplication() {
    Franc five = new Franc(5);
    assertEquals(new Franc(10), five.times(2));
    assertEquals(new Franc(10), five.times(3));
}
```

Franc 클래스가 없으므로 컴파일 에러가 발생한다.

Dollar 클래스를 Franc으로 바꿔보자

```java
public class Franc {
    private int amount;
    
    public Franc(int amount) {
        this.amount = amount;
    }
    
    Franc times(int multiplier) {
        return new Franc(amount * multiplier);
    }
    
    public boolean equals(Object object) {
        Franc franc = (Franc) object;
        return amount == franc.amount;
    }
}
```

테스트가 바로 통과하는 것을 볼 수 있다.

다시 TDD의 주기를 살펴보면

1. 테스트 작성
2. 컴파일되게 하기
3. 실패하는지 확인하기 위해 실행
4. 실행하게 만듦
5. 중복 제거

위 다섯 단계에는 서로 다른 목적이 있다. 다른 스타일의 해법, 다른 미적 시간을 필요로한다.

1~4 단계는 **빨리 진행해야 한다.** 그러면 새 기능이 포함되더라도 잘 알고 있는 상태에 이를 수 있다.

4단계까지 도달하기 위해서라면 어떤 죄(복사 붙여넣기, 상수 넣기 ...)를 저지를 수 있다.

주기의 다섯 번째 단계가 없이는 앞의 네 단계도 제대로 되지 않는다.

적절한 시기에 적절한 설계를. 돌아가게 만들고, 올바르게 만들어라.

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
* 공용 equals
* 공용 times

코드를 실행시키기까지의 단계가 짧았기 때문에 '컴파일되게 하기' 단계도 넘어갈 수 있었다.

5CHF X 2 = 10CHF를 해결했지만 중복 제거라는 할 일이 생겼다.



**검토**

* 큰 테스트를 바로 공략할 수 없다. 그래서 진전을 나타낼 수 있는 자그마한 테스트를 만들었다.(```5CHF * 2 = 10CHF```)
* 중복을 만들고 조금 고쳐서 테스트를 작성했다. (```testFrancMultiplication()```)
* 모델 코드까지 도매금으로 복사하고 수정해서 테스트를 통과했다. (Franc 클래스)