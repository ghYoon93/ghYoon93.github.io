---
title: Test Driven Development [레스토랑 예약 시스템]
categories: [TDD]
tags: [Spring Boot, project, 레스토랑 예약 시스템]
---





테스트 주도 개발에 대해 알아보자

### TDD

테스트 주도 개발로 테스트를 먼저 작성하여 올바르게 작동하고, 깔끔한 코드를 만드는 개발 방법

##### TDD 사이클

Red: 실패하는 테스트, 만들고자 하는 기능의 틀 만들기

Green: 하나의 경우에 성공하는 메서드 만들기

Ractoring: 메서드가 어떤 경우에도 성공할 수 있도록 리팩토링하기



#### ```no tests found for given includes```

gralde에서 junit @Test를 사용하기 위해서 다음과 같은 라이브러리를 추가해야한다. 

~~import.org.junit.Test;~~

import.org.junit.jupiter.api.Test;



#### ```assertThat(T actual, Matcher<? super T> matcher);```

기능의 결과를 테스트할 때 결과(T actual)이 Matcher 메서드(is(), allOf())와 일치하는지 확인하는 메서드