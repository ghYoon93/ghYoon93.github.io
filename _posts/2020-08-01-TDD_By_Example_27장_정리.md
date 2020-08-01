---
title: 테스팅 패턴
categories: TDD
---



> 더 상세한 테스트 작성법에 알아보자



### 자식 테스트

**지나치게 큰 테스트 케이스를 어떻게 돌아가도록 할 수 있을까?**
**\>> 원래 테스트 케이스의 깨지는 부분에 해당하는 작은 테스트 케이스를 작성하고 그 작은 테스트 케이스가 실행되도록 하라. 그 후에 다시 원래의 큰 테스트 케이스를 추가하라.**



빨강/초록/리팩토링 리듬은 성공이 지속되는 데 너무나도 중요해서, 그 리듬을 잃어버릴 것 같은 위기 순간에 **부가의 노력**으로 리듬을 유지하는 것이 충분히 가치 있다.



### 모의 객체

**비용이 많이 들거나 복잡한 리소스에 의존하는 객체를 테스트하려면 어떻게 해야 할까?**
**\>> 상수를 반환하게끔 만든 속임수 버전의 리소스를 만들면 된다.**



모의 객체의 고전적인 예로 데이터베이스가 있다. 

데이터베이스는 시작 시간이 오래 걸리고, 깨끗한 상태로 유지하기가 어렵다. 그리고 만약 데이터베이스가 원격 서버에 있다면 이로 인해 테스트 성공 여부가 네트워크 상의 물리적 위치에 영향을 받게 된다. 또한 데이터베이스는 개발 중 많은 오류의 원인이 된다.

해법은 데이터베이스 대신 데이터베이스인 것처럼 행동하는 객체를 만드는 것이다.

```java
public void testOrderLookUp() {
    Database db = new MockDatabase();
    db.expectQuery("select order_no from Order where cust_no is 123");
    db.returnResult(new String[] {"Order 2", "Order 3"});
}
```

MockDatabase는 예상된 커리를 얻지 못하면 예외를 던질 것이다. 만약 쿼리가 올바르다면 MockDatabase는 상수 문자열에서 마치 결과 집합(result set)처럼 보이는 뭔가를 생성하여 반환한다.

성능과 견고함 이외에 모의 객체의 또 다른 가치는 가독성에 있다. 실제 데이터베이스를 사용한다면 어떤 쿼리가 결과 14개를 되돌려야 한다고 적은 테스트를 보더라도 왜 14개를 결과로 되돌려야 하는게 답인지 알기가 쉽지 않다.



만약 모의 객체를 사용하길 원한다면, 값비싼 자원을 전역 변수에 손쉽게 저장해 버릴 수는 없다.(비록 그것들이 싱글톤의 가면을 썼더라도.) 만약 그렇게 한다면, 전역 변수를 모의 객체로 설정하고, 테스트를 실행한 후 다시 전역 변수를 복구시켜 놓아야한다.

모의 객체는 모든 객체의 가시성에 대해 고민하도록 격려해서 설계에서 커플링이 감소하도록한다. 모의 객체를 사용하념 프로젝트에 위험 요소가 하나 추가된다. 모의 객체가 진짜 객체와 동일하게 동작하지 않으면 어떻게 될까?

모의 객체용 테스트 집합을 진짜 객체가 사용 가능해질 때 그대로 적용해서 이러한 위험을 줄일 수 있다.



### 셀프 션트

한 객체가 다른 객체와 올바르게 대화하는지 테스트하려면 어떻게 할까?
\>> 테스트 대상이 되는 객체가 원래의 대화 상대가 아니라 테스트 케이스와 대화하도록 만들면 된다.

테스팅 사용자 인터페이스의 초록 막대를 동적으로 업데이트하고자 하는 상황을 가졍해보자. 

UI 객체를 TestResult와 연결할 수 있다면 테스트가 실행된 시점, 테스트가 실패한 시점, 전체 테스트 슈트가 시작되고 끝난 시점 등을 통보 받을수 있을 것이다. 그리고 이러한 이벤트들을 통보받으면 인터페이스를 갱신하면 된다.

```java
public void testNotification() {
    TestResult result = new TestResult();
    ResultListener listener = new ResultListener();
    result.addListner(listener);
    WasRun("testMethod").run(result);
    assert listener.count == 1;
}

// 이벤트 통보 횟수를 셀 객체
public class ResultListner(){
    private int count;
    public ResultListener() {
        this.count = 0;
    }
    public void startTest() {
        this.count += 1;
    }
}
```

왜 이벤트 리스너를 위해 별도의 객체를 만들어야 하는 걸까? 그냥 테스트 케이스가 일종의 모의 객체 노릇을 하게 하자.

```java
private int count;
public void testNotification() {
    this.count = 0;
    TestResult result = new TestResult();
    result.addListner(this);
    WasRun("testMethod").run(result);
    assert this.count == 1;
}

public void startTest() {
    this.count += 1;
}
```

통보 횟수가 0이었다가 1이 되었다. 이 순서를 테스트에서 바로 읽어낼 수 있다.

* 어떻게 횟수가 1이 될 수 있을까?
  * 