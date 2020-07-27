---
title: 실패 처리하기
categories: TDD
---



**테스트 프레임워크**

* ~~테스트 메서드 호출하기~~
* ~~먼저 setUp 호출하기~~
* ~~나중에 tearDown 호출하기~~
* 테스트 메서드가 실패하더라도 tearDown 호출하기
* 여러 개의 테스트 실행하기
* ~~수집된 결과를 출력하기~~
* ~~WasRun에 로그 문자열 남기기~~
* **실패한 테스트 보고하기**



실패한 테스트를 발견했을 때 좀더 세밀한 단위의 테스트를 작성해서 올바른 결과를 출력해보자

```java
// TestCaseTest
public void testFailedResultFormatting() {
    TestResult result = new TestResult();
    result.testStarted();
    result.testFailed();
    assert result.summary().equals("1 run, 1 failed");
}
```

testStarted()는 테스트가 시작될 때 testFailed()는 테스트가 실패할 때 보내는 메시지이다.

어떻게 이 메시지들을 보낼까? 일단 메시지를 보내기만 하면 전체가 제대로 동작할 것이라 예상할 수 있다.



```java
// TestResult
int failureCount;
public TestResult() {
    failureCount = 0;
}

public void testFailed() {
    failureCount += 1;
}

public String summary() {
    return runCount + " run, "+failureCount+" failed";
}
```

이제 testFailedResultFormatting 테스트가 통과된다. 이제 testFailed()ㄹㄹ 테스트 메서드에서 던진 예외를 잡을 때 호출해보자

```java
// TestCase
public TestResult run() {
    // this.testMethod();
    TestResult result = new TestResult();
    result.testStarted();
    setUp();
    try {
        Method method = this.getClass().getMethod(name, null);
        method.invoke(this, null);
    } catch (InvocationTargetException e) {
        result.testFailed();
    }
    catch (Exception e) {
        throw new RuntimeException(e);
    }
    tearDown();
    return result;
}
```

테스트 메서드를 호출할 때 InvocationTargetException 에러 발생 시 testFailed를 호출했다.

하지만 setUp메서드에서 문제가 생겼을 시 예외를 잡을 수 없다. 일단 할 일 목록에 적어두자.



**테스트 프레임워크**

* ~~테스트 메서드 호출하기~~
* ~~먼저 setUp 호출하기~~
* ~~나중에 tearDown 호출하기~~
* 테스트 메서드가 실패하더라도 tearDown 호출하기
* 여러 개의 테스트 실행하기
* ~~수집된 결과를 출력하기~~
* ~~WasRun에 로그 문자열 남기기~~
* ~~실패한 테스트 보고하기~~
* setUp 에러를 잡아서 보고하기



**검토**

* 작은 스케일의 테스트가 통과하게 만들었다.
* 큰 스케일의 테스트를 다시 도입했다.
* 작은 스케일의 테스트에서 보았던 메커니즘을 이용하여 큰 스케일의 테스트를 빠르게 통과시켰다.
* 중요한 문제를 발견했는데 이를 바로 처리하기보다는 할일 목록에 적어두었다.