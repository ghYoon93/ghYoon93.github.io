---
title: 셈하기
categories: TDD
---



**테스트 프레임워크**

* ~~테스트 메서드 호출하기~~
* ~~먼저 setUp 호출하기~~
* ~~나중에 tearDown 호출하기~~
* 테스트 메서드가 실패하더라도 tearDown 호출하기
* 여러 개의 테스트 실행하기
* **수집된 결과를 출력하기**
* ~~WasRun에 로그 문자열 남기기~~



테스트 메서드에서 예외가 발생해도 tearDown()이 호출되도록 보장해주는 기능을 구현하자

여러 테스트를 실행하고 그 결과는 다음과 같길 원한다. 5개 테스트가 실행됨, 2개 실패, TestCaseTest.testFooBar-zeroDivide Exception, MoneyTest.testNegation-AssertionError

먼저 TestCase.run()이 테스트 하나의 (나중에 여러개를 처리하도록 할 것이다.) 실행 결과를 기록하는 TestResult 객체를 반환하게 하자

```java
// TestCaseTest
public void testResult() {
    test= new WasRun("testMethod");
    TestResult result = test.run();
    assert result.summary().equals("1 run, 0 failed");
}
public  static void main(String[] args) {
    new TestCaseTest("testResult").run();
}
```

테스트 실행을 TestResult 객체 타입으로 선언, 결과 요약은 "1 run, 0 failed"가 나와야한다.

먼저 테스트를 실행할 수 있을 정도로만 TestResult를 구현하자

```java
// TestResult
public class TestResult {
    public String summary() {
        return "1 run, 0 failed";
    }
}

// TestCase
public TestResult run() {
    // ....
    tearDown();
    return new TestResult();
}
```

이제테스트가 실행된다. 이제 TestResult의 summary()를 조금씩 실체화해보자.

```java
// TestResult
int runCount;
public TestResult() {
    runCount = 0;
}
public testStarted() {
    runCount += 1;
}
public  String summary() {
    return runCount+" run, 0 failed";
}
```

이제 테스트가 실행될 때마다 runCount를 증가시킬 수 있고 이를 summary()에서 확인할 수 있게 되었다.



실패 케이스에 대한 테스트를 만들어보자

```java
//TestCaseTest
public testFailedResult() {
    test = WasRun("testBrokenMethod");
    TestResult result = test.run();
    assert result.summary().equals("1 run, 1 failed");
}

// WasRun
public void testBrokenMethod() throws RuntimeException() {
    throw new RuntimeExeption();
}
```



아직 WasRun.testBrokenMethod에서 던진 예외를 처리하지 않았다.  할 일 목록에 잠시 남겨두자.



**테스트 프레임워크**

* ~~테스트 메서드 호출하기~~
* ~~먼저 setUp 호출하기~~
* ~~나중에 tearDown 호출하기~~
* 테스트 메서드가 실패하더라도 tearDown 호출하기
* 여러 개의 테스트 실행하기
* ~~수집된 결과를 출력하기~~
* ~~WasRun에 로그 문자열 남기기~~
* 실패한 테스트 보고하기



**검토**

* 가짜 구현을 한 뒤 단계적으로 상수를 변수로 바꾸어 실제 구현을 만들었다.
* 또 다른 테스트를 작성했다.
* 테스트가 실패했을 때 좀 더 작은 스케일로 또 다른 테스트를 만들어서 실패한 테스트가 성공하게 만드는 것을 보조할 수 있다.

