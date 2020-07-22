---
title: xUnit으로 가는 첫걸음
categories: TDD
---



2부는 자기 참조(self-referential) 프로그래밍에 대한 전산학 실습

또한 테스트 주도 개발을 통해 '실제' 소프트웨어를 만드는 발전된 예제



### 부트스트랩이란?

**부트스트랩**(bootstrap) 또는 **부트스트래핑**(bootstrapping)은 "현재 상황에서 어떻게든 한다"는 뜻이다. 또, 사물의 초기 단계에서 단순 요소로부터 복잡한 체계를 구축하는 과정을 가리키는 경우도 있다.

* 부트스트랩 (컴퓨팅): 더 복잡한 도구를 만들 수 있도록 도와 주는 단순 도구를 만들거나 적재함으로써 복잡한소프트웨어 도구를 만들거나 컴퓨터를 시작하는 것을 말한다. 줄여서 시동이라고도 할 수 있으며, 이는 컴퓨터를 시작하는 과정을 서술해 준다.

- [부트스트랩 (컴파일러)](https://ko.wikipedia.org/wiki/부트스트랩_(컴파일러)): 언어 그 자체를 사용하여 컴퓨터 언어에 맞춰 컴파일을 기록하는 것을 말한다.



**테스트 프레임워크**

* **테스트 메서드 호출하기**
* 먼저 setUp 호출하기
* 나중에 tearDown 호출하기
* 테스트 메서드가 실패하더라도 tearDown 호출하기
* 여러 개의 테스트 실행하기
* 수집된 결과를 출력하기



**첫번째 원시 테스트**

* 테스트 메서드가 호출되면 true, 그렇지 않으면 false를 반환



**부트스트랩 테스트를 위한 전략**

* 플래그를 가지고 테스트가 통과되는 지 확인
* 테스트를 실행하기 전에 플래그는 false 실행된 후에 플래그는 true



```java
WasRun test = new WasRun("testMethod");
System.out.println(test.wasRun);
// test.testMethod();
test.run();
System.out.println(test.wasRun);

// WasRun
class WasRun {
    
    boolean wasRun;
    
    public WasRun(String name) {
        this.wasRun = false;    
    }
    
    public void testMethod() {
        this.wasRun = true;
    }
    
    public void run() {
        this.testMethod();
    }
}
```



테스트 객체가 testMethod를 직접 호출하는 대신 인터페이스인 run을 만들어 호출하도록 했다.



testMethod()를 동적으로 호출하도록 리팩토링 (테스트 케이스의 이름과 같은 문자열을 갖는 필드가 주어지면, 함수로 호출될 때 해당 메서드를 호출하게끔 하는 객체 얻어내기)

```java
// WasRun
public void run() {
    // this.testMethod();
    try {
        Method method = this.getClass().getMethod(name, null);
        method.invoke(this, null);
    } catch (Exception e) {
        throw new RuntimeException(e);
    }
}
```



하나의 특별한 사례에 대해서만 작동하는 코드를 가져다가 다른 여러 사례에 대해서도 작동할 수 있도록 상수를 변수로 변화시켜 일반화하는 것은 리팩토링의 일반적인 패턴

머릿 속 순수한 추론에 의해 일반화하게 하지 않고, 잘 돌아가는 구체적인 사례에서 시작하여 일반화할 수 있게 해준다는 점에서 TDD는 이 패턴을 잘 지원한다는 것을 알 수 있다.



WasRun이 하는 두가지 독립적인 일

* 메서드가 호출되었는지 그렇지 않은지를 기억
* 메서드를 동적으로 호출하는 일



유사분열을 일으킬 시기이다. (두 부분을 서로 분리했고 다시 원래대로 하나로 돌아가지 않는다면 상위 클래스를 만드는 것을 고려)

```java
// TestCase
public class TestCase {
    String name;
    public TestCase(String name) {
        this.name = name;
    }
}
```



```java
// TestCase
public class TestCase {
    private String name;
    public TestCase(String name) {
        this.name = name;
    }
    public void run() {
        // this.testMethod();
        try {
            Method method = this.getClass().getMethod(name, null);
            method.invoke(this, null);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}

// WasRun
boolean wasRun;
    
    public WasRun(String name) {
        super(name);
        this.wasRun = false;
    }
    
    public void testMethod() {
        this.wasRun = true;
    }
}

// TestCaseTest
public class TestCaseTest extends TestCase {
    
    public TestCaseTest(String name) {
        super(name);
    }
    
    public void testRunning() {
        WasRun test = new WasRun("testMethod");
        System.out.println(test.wasRun);
        assert test.wasRun == false;
        // test.testMethod();
        test.run();
        System.out.println(test.wasRun);
        assert test.wasRun == true;

    }

    public static void main(String[] args) {
        new TestCaseTest("testRunning").run();
    }

}
```

run 메서드와 name 변수를 TestCase로 끌어올렸다.  (오퍼레이션을 데이터 근처에 놓을 방법을 고민해보자)

이후 WasRun과 TestCaseTest에서 TestCase를 상속받음으로써 run 메서드를 호출할 수 있도록 했다.





**테스트 프레임워크**

* ~~테스트 메서드 호출하기~~
* 먼저 setUp 호출하기
* 나중에 tearDown 호출하기
* 테스트 메서드가 실패하더라도 tearDown 호출하기
* 여러 개의 테스트 실행하기
* 수집된 결과를 출력하기



**검토**

* 몇 번의 잘못된 출발을 한 후, 아주 자그마한 단계로 시작하는 법을 알아냈다.
* 일단 하드코딩을 한 다음에 상수를 변수로 대체하여 일반성을 이끌어내는 방식으로 기능을 구현
* 플러거블 셀렉터(Pluggable Selector)를 사용했다. 플러거블 셀렉터는 정적 코드 분석을 어렵게 만든다.
* 테스트 프레임워크를 작은 단계로만 부트스트랩했다.