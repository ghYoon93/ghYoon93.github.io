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



```python
test = WasRun("testMethod")
print test.wasRun
test.testMethod()
print test.wasRun

# WasRun
class WasRun:
    def __init__(self, name):
        self.wasRun = None
    
    def testMethod(self):
        self.wasRun = 1
    
    def run(self):
        self.testMethod()
```

