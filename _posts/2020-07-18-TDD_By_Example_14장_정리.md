---
title: 바꾸기
categories: TDD
---



**통화가 다른 두 금액을 더해서 주어진 환율에 맞게 변한 금액을 결과로 얻을수 있어야한다.**

* $5 + 10CHF = $10
* $5 + $5 = $10
* $5 + $5에서 Money 반환하기
* ~~Bank.reduce(Money)~~
* **Money에 대한 통화 변환을 수행하는 Reduce**
* Reduce(Bank, String)



> 프랑을 달러로 바꿔보자

```java
// MoneyTest
public void testReduceMoneyDifferentCurrency() {
    Bank bank = new Bank();
    bank.addRate("CHF", "USD", 2);
    Money result = bank.reduce(Money.franc(2), "USD");
    assertEquals(Money.dollar(1), result);
}
```

bank는 환율을 추가하고 (1달러는 2프랑) 2프랑을 1달러로 바꿨다. 그 결과는 1달러와 같아야한다.



```java
// Money
public Money reduce(String to) {
    int rate = (currency.equals("CHF") && to.equals("USD"))
        ? 2
        : 1;
    return new Money(amount / rate, to);
}

```

갑자기 Money가 환율을 처리하고 있다. 이는 책임에 어긋난다. (환율은 Bank가 처리해야하고 Money가 환율을 알고 싶으면 Bank를 통해 알아야한다.)

이제 Expression의 reduce에서 인자로 Bank를 넘겨야한다.

```java
// Bank
Money reduce(Expression source, String to) {
    // return source.reduce(to);
    return source.reduce(this, to);
}
// Expression
Money reduce(Bank bank, String to);

// Sum
public Money reduce(Bank bank, String to) {
    int amount = augend.amount + addend.amount;
    return new Money(amount, to);
}

// Money
public Money reduce(Bank bank, String to) {
    int rate = (currency.equals("CHF") && to.equals("USD"))
        ? 2
        : 1;
    return new Money(amount / rate, to);
}
```



이제 Bank에서 환율을 계산하자

``` java
// Bank
int rate(String from, String to) {
    return (from.equals("CHF") && to.equals("USD"))
        ? 2
        : 1;
}
```



환율을 설정할 때 상수 2를 없앨 차례다. 이걸 없애려면 Bank에서 환율표를 가지고 있다가 필요할 때 찾아볼 수 있게 해야한다. 두 개의 통화와 환율을 매핑시키는 해시 테이블을 사용할 수 있겠다. 통화 쌍을 해시 테이블의 키로 쓰기 위해 배열을 사용할 수 있는 지 알아보자.

```java
public void testArrayEquals() {
    assertArrayEquals(new Object{} {"abc"}, new Object[] {"abc"});
}
```

**테스트가 실패한다?**



```java
// Pair
private class Pair {
    private String from;
    private String to;

    Pair(String from, String to) {
        this.from = from;
        this.to = to;
    }

    public boolean equals(Object object) {
        Pair pair = (Pair) object;
        return from.equals(pair.from)&& to.equals(pair.to); 
    }

    public int hashCode() {
        return 0;
    }
}
```

Pair은 Bank의 내부 클래스로 만들었고 Pair을 키로 쓰기 위해 equals()와 hashCode()를 구현했다.

0은 최악의 해시 코드지만 구현하기 쉽고 빠르게 달리기 위해 썼다. (해시 테이블에서 검색이 선형 검색과 비슷하게 수행될 것이다.)

나중에 많은 통화를 다뤄야 할 일이 생기면 그 때 실제 측정 데이터를 가지고 개선하게 될 것이다.

```java
// Bank
private Hashtable<Pair, Integer> rates = new Hashtable<Pair, Integer>(); // 환율 저장

void addRate(String from, String to, int rate) {
    rates.put(new Pair(from, to), new Integer(rate));
}  // 환율 설정


int rate(String from, String to) {
    // return (from.equals("CHF") && to.equals("USD"))
    //     ? 2
    //     : 1;
    Integer rate = (Integer) rates.get(new Pair(from, to));
    return rate.intValue();
}  // 환율 구하기
```



환율에 대한 구현을 마치고 테스트를 해봤더니 testReduceMoney에서 테스트가 실패했다.

원인은 환율을 구할 때 같은 통화에 대한 환율을 설정하지 않았기 때문이다.

이에 대해 같은 통화의 환율을 구할 때 1을 반환하는 대한 테스트를 만들자

```java
public void testIdentityRate() {
    assertEquals(1, new Bank().rate("USD", "USD"));
}

// Bank
int rate(String from, String to) {
    if(from.equals(to)) return 1;
    Integer rate = (Integer) rates.get(new Pair(from, to));
    return rate.intValue();
}
```

