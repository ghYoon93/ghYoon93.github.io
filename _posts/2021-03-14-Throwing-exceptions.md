---
title: '예외 던지기'
excerpt: '예외의 언제 어떻게 던져야하는지 알아보자'

categories:
  - Java
tags:
  - Errorless code
  - Exception handling

toc: true
toc_label: 'Table of contents'
toc_sticky: true
---

> 출처 [Throwing exceptions - JetBrains Academy](https://hyperskill.org/learn/step/3553)를 보고 요약한 내용입니다.

## 1. The throw keyword

`Throwable`과 서브클래스는 **throw** 구문으로 던져질 수 있습니다.

**throw 구문의 일반적인 형식**
`throw 던져질 객체`

**`RuntimeException` 던지기**

```java
public class Main {
    public static void main(String[] args) {
        RuntimeException exception =
            new RuntimeException("Something's bad");
        throw exception;
    }
}
```

**결과**

```java
Exception in thread "main" java.lang.RuntimeException: Something's bad.
    at Main.main(Main.java:3)
```

**한줄로 예외 객체를 생성하고 던지는 방법**

- `Throwable`의 인스턴스

  ```java
  throw new Throwable("Something's bad");
  ```

- `Exception`의 인스턴스

  ```java
  throw new Exception("Something's bad");
  ```

- `NullPointerException`의 인스턴스

  ```java
  throw new NullPointerException("The field is null");
  ```

> `Throwable`을 상속 받지 않는 클래스의 객체를 던지는 것은 불가능합니다.

## 2. Throwing checked exceptions

메서드가 확인되는 예외를 포함하고 있으면 메서드 선언 뒤 `throws`를 특정해야합니다.

**여러 예외 선언하기**

`,`로 예외 클래스를 분리하여 선언

```java
public static void method() throws ExceptionType1, ExceptionType2, ExceptionType3
```

메서드가 상위 예외를 던진다고 선언되면 메서드는 메서드는 특정 예외의 모든 서브클래스를 던질 수 있습니다.

## 3. Throwing unchecked exceptions

확인되지 않는 예외는 메서드 선언 시 `throws` 키워드를 필요로 하지 않습니다. 그 외는 확인되는 예외와 같습니다.

## 4. When to throw an exception?

항상 명확하지는 않지만 일반적으로는 메서드 전제 조건을 만족하지 않을 때, 즉 현재 조건에서 코드를 수행할 수 없는 경우에만 예외를 던집니다.

**예시**

- 메서드가 파일을 읽어야하지만, 파일이 존재하지 않을 때(`FileNotFoundException`)
- 메서드가 주어진 문자열에서 월을 분리해야하지만 문자열이 유효하지 않을 때 (`InvalidArgumentException`)

예외를 던질 때 기본 `Exception` 클래스를 던지는 것보다 특정한 서브클래스를 던지는 것을 좋습니다.
