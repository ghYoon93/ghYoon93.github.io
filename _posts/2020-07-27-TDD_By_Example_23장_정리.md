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