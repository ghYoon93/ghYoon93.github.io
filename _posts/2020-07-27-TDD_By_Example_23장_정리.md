---
title: 얼마나 달콤한지 (How Suite It Is)
categories: TDD
---



**테스트 프레임워크**

* ~~테스트 메서드 호출하기~~
* ~~먼저 setUp 호출하기~~
* ~~나중에 tearDown 호출하기~~
* 테스트 메서드가 실패하더라도 tearDown 호출하기
* **여러 개의 테스트 실행하기(23)**
* ~~수집된 결과를 출력하기~~
* ~~WasRun에 로그 문자열 남기기~~
* ~~실패한 테스트 보고하기~~
* setUp 에러를 잡아서 보고하기



TestSuite를 다루지 않고는 xUnit을 떠날 수 없다. 파일의 끝 부분에는 모든 테스트들을 호출하는 코드가 있는데, 좀 비참해보인다.

```java
public static void main(String[] args) {
    System.out.println(new TestCaseTest("testTemplateMethod").run().summary());
    System.out.println(new TestCaseTest("testResult").run().summary());
    System.out.println(new TestCaseTest("testFailedResultFormatting").run().summary());
    System.out.println(new TestCaseTest("testFailedResult").run().summary());
}
```

놓친 디자인 요소를 찾기 위해 일부러 만드는 중복이 아니라면, 중복은 언제나 나쁘다. 테스트들을 모아서 한 번에 실행할 수 있는 기능을 원한다(테스트를 한 번에 하나씩만 실행시킨다면 테스트가 독립적으로 돌아가도록 만들기 위해 고생하는 건 별 의미가 없다). 

TestSuite를 구현할 때 컴포지트 패턴를 접해볼 수 있다.

TestSuite를 만들고 거기에 테스트를 몇 개 넣은다음 이것을 모두 실행하고 그 결과를 얻어내자



```java
//TestCaseTest
public void testSuite() {
        
    TestSuite suite = new TestSuite();
    suite.add(new WasRun("testMethod"));
    suite.add(new WasRun("testBrokenMethod"));

    TestResult result = suite.run();

    assert result.summary().equals("2 run, 1 failed");
}

// TestSuite
ArrayList<TestCase> tests;
    
public TestSuite() {
    tests = new ArrayList<TestCase>();
}

public void add(TestCase testCase) {
    tests.add(testCase);
}
```

add 메서드는 그냥 테스트를 리스트에 추가하는 작업만 한다.

하나의 TestResult가 모든 테스트에 대해 쓰이고자 하면 run 메서드는 다음과 같이 구현해야한다.

```java
// TestSuite
public TestResult run() {
    TestResult result = new TestResult();
    for(WasRun test : tests) {
        test.run(result);
    }
    return result;
}
```

foreach문으로 tests 리스트의 각 요소를 실행할 수 있다. 하지만 컴포지트의 주요 제약 중 하나는 **컬렉션이 하나의 개별 아이템인 것처럼 반응해야한다는 것이다.** 만약 TestCase.run()에 매개 변수를 추가하게 되면 TestSuite.run()에도 똑같은 매개 변수를 추가해야한다.

이에 세가지 대안이 있다.

1. 기본 매개 변수 기능 사용 -> 기본값은 런타임이 아닌 컴파일타임에 평가되므로 하나의 TestResult를 재사용 할 수 없다.
2. 메서드를 두 부분으로 나눈다. 하나는 TestResult를 할당하는 부분, 하나는 할당된 TestResult를 가지고 테스트를 하는 부분 -> 두 부분에 대한 좋은 이름이 떠오르지 않고 이는 두 부분으로 나누는 것은 좋은 전략이 아니라는 것을 뜻한다.
3. 호출하는 곳에서 TestResults를 할당한다.

호출하는 곳에서 TestResults를 할당하는 전략을 변수 수집(collecting parameter) 패턴이라 한다.

```java
// TestCaseTest
public void testSuite() {
        
    TestSuite suite = new TestSuite();
    suite.add(new WasRun("testMethod"));
    suite.add(new WasRun("testBrokenMethod"));

    TestResult result = new TestResult();
    suite.run(result);

    assert result.summary().equals("2 run, 1 failed");
}

// TestSuite
public TestResult run(TestResult result) {
    for(TestCase test : tests) {
        test.run(result);
    }
    return result;
}

// TestCase
public TestResult run(TestResult result) {
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



이제 테스트 호출 코드를 정리할 수 있다.

```java
// TestCaseTest
public static void main(String[] args) {
    TestSuite suite = new TestSuite();
    suite.add(new TestCaseTest("testTemplateMethod"));
    suite.add(new TestCaseTest("testResult"));
    suite.add(new TestCaseTest("testFailedResultFormatting"));
    suite.add(new TestCaseTest("testFailedResult"));
    TestResult result = new TestResult();
    suite.run(result);
    System.out.println(result.summary());
}
```



**테스트 프레임워크**

* ~~테스트 메서드 호출하기~~
* ~~먼저 setUp 호출하기~~
* ~~나중에 tearDown 호출하기~~
* 테스트 메서드가 실패하더라도 tearDown 호출하기
* **여러 개의 테스트 실행하기(23)**
* ~~수집된 결과를 출력하기~~
* ~~WasRun에 로그 문자열 남기기~~
* ~~실패한 테스트 보고하기~~
* setUp 에러를 잡아서 보고하기
* TestCase클래스에서 TestSuite 생성하기



중복이 상당히 많은데, 주어진 테스트 클래스에 대한 테스트 슈트를 자동 생성할 방법이 있다면 그 중복을 제거할 수 있을 것이다.

하지만 우선 run메서드 리팩토링으로 인해 컴파일 에러가 발생하는 테스트 세 개를 고쳐야한다.

```java
// TestCaseTest
public void testTempleteMethod() {
    TestResult result = new TestResult();
    test.run(result);
}
public void testResult() {
    TestResult result = new TestResult();
    test.run(result);
}
public void testFailedResult() {
    TestResult result = new TestResult();
    test.run(result);
}
// TestCaseTest
public static void main(String[] args) {
    TestSuite suite = new TestSuite();
    suite.add(new TestCaseTest("testTemplateMethod"));
    suite.add(new TestCaseTest("testResult"));
    suite.add(new TestCaseTest("testFailedResultFormatting"));
    suite.add(new TestCaseTest("testFailedResult"));
    TestResult result = new TestResult();
    suite.run(result);
    System.out.println(result.summary());
```

각 테스트들이 TestResult를 할당하고 있다. 이것은 setUp으로 해결할  수 있는 문제다. TestResult를 setUp()에서 생성하게 만들면 테스트를 단순화할 수 있다.

```java
// TestCaseTest
TestResult result;
public void setUp(){
    result = new TestResult();
}
```





**테스트 프레임워크**

* ~~테스트 메서드 호출하기~~
* ~~먼저 setUp 호출하기~~
* ~~나중에 tearDown 호출하기~~
* 테스트 메서드가 실패하더라도 tearDown 호출하기
* ~~여러 개의 테스트 실행하기(23)~~
* ~~수집된 결과를 출력하기~~
* ~~WasRun에 로그 문자열 남기기~~
* ~~실패한 테스트 보고하기~~
* setUp 에러를 잡아서 보고하기
* TestCase클래스에서 TestSuite 생성하기



**검토**

* TestSuite를 위한 테스트를 작성했다.
* 테스트를 통과시키지 못한 채 일부분만 구현했다. 이것은 '규칙' 위반이다. 만약 그때 이걸 직접 발견했다면, 돈주머니에서 테스트 케이스 두 개를 공짜로 가져가도 좋다. 테스트를 통과시키고 초록 막대 상태에서 리팩토링할 수 있게 할 간단한 가짜 구현이 있을 것 같긴한데, 지금은 그게 뭔지 잘 떠오르지 않는다.
* 아이템과 아이템의 모음(컴포지트)이 동일하게 작동할 수 있도록 run메서드의 인터페이스를 변경했고 마침내 테스트를 통과시켰다.
* 공통된 셋업 코드를 분리했다.