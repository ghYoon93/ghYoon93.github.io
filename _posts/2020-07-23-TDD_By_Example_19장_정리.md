---
title: 테이블 차리기
categories: TDD
---



**테스트 작성 시 공통된 패턴**

1. 준비(arrange) - 객체를 생성한다.
2. 행동(act) - 어떤 자극을 준다.
3. 확인 (assert) - 결과를 검사한다. 



**테스트 프레임워크**

* ~~테스트 메서드 호출하기~~
* **먼저 setUp 호출하기**
* 나중에 tearDown 호출하기
* 테스트 메서드가 실패하더라도 tearDown 호출하기
* 여러 개의 테스트 실행하기
* 수집된 결과를 출력하기



행동과 확인 단계는 각 테스트마다 다르지만 준비 단계는 여러 테스트에 걸쳐 동일한 경우가 종종 있다.



테스트마다 객체 생성 시 두가지 제약이 상충한다.

* 성능 - 여러 테스트에서 같은 객체를 사용한다면, 객체 하나만 생성해서 모든 테스트가 이 객체를 쓰게 할 수 있다.
* 격리 - 어떤 테스트의 성공 여부가 다른 테스트에 영향을 주면 안된다. 만약 테스트들이 객체를 공유하는 상태에서 어떤 테스트가 객체의 상태를 변경한다면 다른 테스트의 결과에 영향을 미칠 수 있다.



테스트 사이의 커플링을 만들지 말 것 - 테스트 사이의 커플링은 실패하는 테스트를 통과시킬 수 있고 반대로 통과하는 테스트를 실패시킬 수 있다.



```java
// TestCase
protected void setUp(){}

public void run() {
    setUp();
    try{
        try {
            Method method = this.getClass().getMethod(name, null);
            method.invoke(this, null);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}

// WasRun
protected void setUp() {
    this.wasRun = false;
    this.wasSetUp = true;
}

// TestCaseTest
WasRun test;

protected void setUp() {
    test = WasRun("testMethod");
}
public void testRunning() {
    test.run();
    assert test.wasRun == true;
}

public void testSetUp() {
    test.run();
    assert test.wasSetUp == true;
}
```



**테스트 프레임워크**

* ~~테스트 메서드 호출하기~~
* ~~먼저 setUp 호출하기~~
* 나중에 tearDown 호출하기
* 테스트 메서드가 실패하더라도 tearDown 호출하기
* 여러 개의 테스트 실행하기
* 수집된 결과를 출력하기





**검토**

* 일단 테스트를 작성하는 데 있어 간결함이 성능 향상보다 더 중요하다고 생각하자
* setUp()을 테스트하고 구현했다.
* 예제 테스트 케이스를 단순화하기 위해 setUp()을 사용했다.
* 예제 테스트 케이스에 대한 테스트 케이스를 단순화하기 위해 setUp()을 사용했다.