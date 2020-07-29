---
title: 테스트 주도 개발 패턴
categories: TDD
---





* 테스트한다는 것은 무엇을 뜻하는가
* 테스트를 언제 해야하는가
* 테스트할 로직을 어떻게 고를 것인가
* 테스트할 데이터를 어떻게 고를 것인가



# 테스트를 한다는 것은 무엇을 뜻하는가

### 테스트(명사)

작성한 소프트웨어를 어떻게 테스트할 것인가? 자동화된 테스트를 만들어라.

### 격리된 테스트

테스트를 실행하는 것이 서로 어떤 식으로 영향을 미쳐야 좋은가? 아무 영향이 없어야 한다.

테스트는 전체 애플리케이션을 대상으로 하는 것보다 좀 더 작은 스케일로 하는 것이 좋다.



### 테스트 목록

뭘 테스트해야 하나? 시작하기 전에 작성해야 할 테스트 목록을 모두 적어둘 것.



# 테스트는 언제 해야하는가

### 테스트 우선

테스트를 언제 작성하는 것이 좋을까? 테스트 대상이 되는 코드를 작성하기 직전에 작성하는 것이 좋다.



# 테스트할 로직을 어떻게 고를 것인가

### 단언 우선

테스트를 작성할 때 단언은 언제쯤 쓸까? 단언을 제일 먼저 쓰고 시작하라. 

* 시스템을 개발할 때 무슨 일부터 하는가? 안료된 시스템이 어떨 거라고 알려주는 이야기부터 작성한다.
* 특정 기능을 개발할 때 무슨 일부터 하는가? 기능이 완료되면 통과할 수 있는 테스트부터 작성한다.
* 테스트를 개발할 때 무슨 일부터 하는가? 완료될 때 통과해야할 단언부터 작성한다.



단언을 먼저 작성하면 작업을 단순하게 만드는 강력한 효과가 있다. 
구현에 대해 전혀 고려하지 않고 테스트만 작성할 때도 다음과 같은 문제들을 한번에 해결할 수 있다.

* 테스트하고자 하는 기능이 어디에 속하는 걸까? 기존 메서드를 수정해야하나, 기존 클래스에 새로운 메서드를 추가해야 하나, 아니면 이름이 같은 메서드를 새 장소 또는 새 클래스에?
* 메서드 이름을 뭐라고 해야 하나?
* 올바른 결과를 어떤 식으로 검사할 것인가?
* 이 테스트가 제안하는 또 다른 테스트에는 뭐가 있을까?



"올바른 결과는 무엇인가?", "어떤 식으로 검사할 것인가?"는 나머지 문제에서 쉽게 분리할 수 있다.

소켓을 통해 다른 시스템과 통신하려 한다고 가정하자. 통신을 마친 후 소켓은 닫혀있고, 소켓에서 문자열 'abc'를 읽어와야 한다고 해보자

```java
public void testCompleteTransaction() {
    // ...
    assertTrue(reader.isClosed());
    assertEquals("abc", reply.contents());
}
```

위의 단언에서 우리는 reader가 소켓과 관련이 있고 reply는 통신 시 받아온 데이터라고 유추해볼 수 있다.

```java
public void testCompleteTransaction() {
    // ...
    Buffer reply = reader.contents();
    assertTrue(reader.isClosed());
    assertEquals("abc", reply.contents());
}
```

reader에서 reply를 얻어왔다. 그럼 reader는 어디서 얻어올까?

```java
public void testCompleteTransaction() {
    // ...
    Socket reader = Socket("localhost", defaultPort());
    Buffer reply = reader.contents();
    assertTrue(reader.isClosed());
    assertEquals("abc", reply.contents());
}
```

서버에 접속해서 reader를 얻어왔다. 서버를 접속하려면 서버가 열려있어야한다.

```java
public void testCompleteTransaction() {
	Server writer = Server(defaultPort(), "abc");
    Socket reader = Socket("localhost", defaultPort());
    Buffer reply = reader.contents();
    assertTrue(reader.isClosed());
    assertEquals("abc", reply.contents());
}
```

용도에 맞게 이름을 수정하는 일을 해야하지만 아주 작은 단계로 빠른 피드백을 받으며 테스트의 아웃라인을 만들었다.



# 테스트할 데이터를 어떻게 고를 것인가

### 테스트 데이터

테스트할 때 어떤 데이터를 사용해야 하는가? 테스트를 읽을 때 쉽고 따라가기 좋을 만한 데이터를 사용하라.

테스트 데이터 패턴이 완전한 확신을 얻지 않아도 되는 라이센스는 아니다. 만약 시스템이 여러 입력을 다루어야 한다면 테스트  역시 여러 입력을 반영해야한다.

테스트 데이터 패턴의 한 가지 트릭은 여러 의미를 담는 동일한 상수를 쓰지 않는 것이다.

테스트 데이터에 대한 대안은 실제 세상에서 얻어진 실제 데이터를 사용하는 것이다.

실제 데이터는 다음과 같은 경우에 유용하다.

* 실제 실행을 통해 수집한 외부 이벤트의 결과를 이용하여 실시간 시스템을 테스트하고자 하는 경우.
* 예전 시스템의 출력과 현재 시스템의 출력을 비교하고자 하는 경우(병렬 테스팅).
* 시뮬레이션 시스템을 리팩토링한 후 기존과 정확히 동일한 결과가 나오는지 확인하고자 할 경우. 특히 부동소수점 값의 정확성이 문제가 될 수 있다.



### 명백한 데이터

데이터의 의도를 어떻게 표현할 것인가? 테스트 자체에 예상되는 값과 실제 값을 포함하고 이 둘 사이의 관계를 드러내기 위해 노력하라.



"한 통화를 다른 통화로 환전하려고 하는데, 이 거래에는 1.5%의 수수료가 붙는다. USD에서 GBP로 교환하는 환율이 2:1이라면 $100를 환전하면 50GBP - 1.5% = 49.25GBP를 받는다." 라는 테스트를 작성해보자.

```java
public void testExample() {

    Bank bank = new Bank();
    bank.addRate("USD", "GBP", STANDARD_RATE);
    bank.commission(STANDARD_COMMISSION);
    
    Money result = bank.convert(new Note(100, "USD"), "GBP");
    
    assertEquals(new Note(49.25, "GBP"), result);
}

```

USD에서 GBP로 교환하는 환율이 STANDARD_RATE고 수수료가 STANDARD_COMMISION이고 convert() 메서드가 환전 결과를 뜻하는 것을 "유추"할 수 있다.

나쁘지 않은 테스트지만 더 명확히 표현해보자.

```java
public void testExample() {

    Bank bank = new Bank();
    bank.addRate("USD", "GBP", 2);
    bank.commission(0.015);
    
    Money result = bank.convert(new Note(100, "USD"), "GBP");
    
    assertEquals(new Note(100/2 * (1-0.015), "GBP"), result);
}

```

이제 환율이 2:1이며 교환 수수료는 1.5%라는 것을 명확히 알 수 있고 입력으로 사용된 숫자와 예상되는 결과사이의 관계를 읽어낼 수 있다.

명백한 데이터가 주는 또 다른 이점은 프로그래밍이 더 쉬워진다는 것이다.

단언 부분에 수식을 써놓으면 다음으로 무엇을 해야 할지 쉽게 알게 된다.

명백한 데이터는 코드에 매직넘버를 쓰지 말라는 것에 대한 예외적인 규칙일 수 있다.