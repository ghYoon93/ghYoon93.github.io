---
title: 뒷정리하기
categories: TDD
---



**테스트 프레임워크**

* ~~테스트 메서드 호출하기~~
* ~~먼저 setUp 호출하기~~
* **나중에 tearDown 호출하기**
* 테스트 메서드가 실패하더라도 tearDown 호출하기
* 여러 개의 테스트 실행하기
* 수집된 결과를 출력하기



테스트 전에 setUp에서 외부 자원 할당했다면 테스트를 마친 후 이 자원을 할당해야 테스트들은 독립적일 수 있다.

이 때 자원을 반환하는 메서드가 tearDown이다. tearDown을 테스트하는 방법은 여태까지 했던 것과 마찬가지로 wasTearDown 플래그를 도입하는 방법이 있다. 

_하지만 플래그를 사용하는 방법은 메서드의 한가지 중요한 면을 놓치고 있다._ setUp은 테스트 메서드가  실행되기 전, tearDown은 실행 후에 호출되어야 한다는 점이다.

플래그 대신 로그를 간단히 남기는 식으로 고쳐보자



**테스트 프레임워크**

* ~~테스트 메서드 호출하기~~
* ~~먼저 setUp 호출하기~~
* 나중에 tearDown 호출하기
* 테스트 메서드가 실패하더라도 tearDown 호출하기
* 여러 개의 테스트 실행하기
* 수집된 결과를 출력하기
* **WasRun에 로그 문자열 남기기**



```java
// WasRun
String log;
protected void setUp() {
    log = "setUp ";
}
public void testMethod() {
    log += "testMethod ";
}

// TestCaseTest
protected void testTemplateMethod() {
    test = new WasRun("testMethod");
    test.run();
    assert test.log.equals("setUp testMethod ");
}
```

먼저 log 인스턴스 변수로 setUp과 wasRun에 관련된 플래그를 대체했다.

하나의 변수만으로 메서드가 호출되었는지 간단하게 확인이 가능하다.

그 다음 TestCaseTest 클래스에서 TestSetUp이 testRunning가 할 테스트도해줌으로 두 메서드를 통합했다.



**테스트 프레임워크**

* ~~테스트 메서드 호출하기~~
* ~~먼저 setUp 호출하기~~
* **나중에 tearDown 호출하기**
* 테스트 메서드가 실패하더라도 tearDown 호출하기
* 여러 개의 테스트 실행하기
* 수집된 결과를 출력하기
* ~~WasRun에 로그 문자열 남기기~~



```java
//WasRun
protected void tearDown() {
    log += "tearDown ";
}

//TestCase
public void run() {
    setUp();
    try {
        Method method = this.getClass().getMethod(methodName, null);
        method.invoke(this, null);
    } catch (Exception e) {
        throw new RuntimeException(e);
    }
    tearDown();
    }
}

protected void setUp(){}
protected void tearDown(){}
```

tearDown 구현은 매우 간단했다. log에 tearDown이 호출되었다는 로그를 남기고 TestCase의 run메서드에서  테스트할 메서드 호출 후 tearDown을 호출하기만 하면 끝이다.

**테스트 프레임워크**

* ~~테스트 메서드 호출하기~~
* ~~먼저 setUp 호출하기~~
* ~~나중에 tearDown 호출하기~~
* 테스트 메서드가 실패하더라도 tearDown 호출하기
* 여러 개의 테스트 실행하기
* 수집된 결과를 출력하기
* ~~WasRun에 로그 문자열 남기기~~



**검토**

* 플래그에서 로그로 테스트 전략을 구조 조정
* 새로운 로그 기능을 이용하여 tearDown()을 테스트하고 구현했다.
* 문제를 발견했는데 뒤로 되돌아가는 대신 과감히 수정



