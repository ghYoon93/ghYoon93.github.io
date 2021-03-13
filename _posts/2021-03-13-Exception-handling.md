---
title: '예외 처리'
excerpt: '예외 처리 메커니즘과 try, catch, finally를 알아보자'

categories:
  - Java
tags:
  - Errorless code
  - Exception handling

toc: true
toc_label: 'Table of contents'
toc_sticky: true
---

> 출처 [Exception handling - JetBrains Academy](https://hyperskill.org/learn/step/3552)를 보고 요약한 내용입니다.

자바는 **확인된 예외와 확인되지 않은 예외** 모두에서 작동하는 **예외 처리** 메커니즘을 제공합니다.

**예외 처리 메커니즘**

- 예외 발생 시 적합한 핸들러 찾기
- 핸들러를 찾으면 실행하고 예외가 처리된 것으로 간주

**핸들러의 위치**

- 예외가 발생한 메서드
- 예외가 발생한 메서드를 호출하는 메서드

`try`, `catch`, `finally`는 예외를 처리하는 키워드로 프로그램이 중단되지 않게 할 수 있습니다.

## 1. The try-catch statement

**에러 처리를 위한 간단한 `try-catch` 템플릿**

```java
try {
    // code that may throw an exception
} catch (Exception e) {
    // code for handling the exception
}
```

**`try`**

예외를던질 수도 있는 코드를 감쌉니다. 메소드 호출을 포함한 모든 코드를 포함할 수 있습니다.

**`catch`**

예외와 예외의 모든 서브 클래스의 특정 타입을 위한 핸들러입니다. `try`에서 해당 유형의 예외가 발생하면 실행됩니다.

`catch` 안의 특정한 타입은 `Throwable` 클래스를 상속받아야합니다.

**`try`와 `catch`의 흐름**

```java
System.out.println("before the try-catch block");

try {
    System.out.println("inside the try block before an exception");

    System.out.println(2 / 0); // ArithmeticException

    System.out.println("inside the try block after the exception");
} catch (Exception e) {
    System.out.println("Division by zero!");
}

System.out.println("after the try-catch block");
```

**결과**

```no-highlight
before the try-catch block
inside the try block before an exception
Division by zero!
after the try-catch block
```

`try` 안에서 예외가 발생하면 발생 이후의 블럭 내 코드는 실행되지 않고 `catch` 안의 코드가 실행됩니다. `catch` 안의 코드가 모두 실행되면 다시 `try`로 돌아가지 않고 다음 구문을 실행합니다.

`Exception`을 `ArithmeticException`이나 `RuntimeException`으로 대체해도 `catch`의 프로그램 예외 흐름은 변하지 않습니다. 하지만 `NumberFormatException` 같은 적합하지 않은 핸들러로 대체하면 프로그램은 실행에 실패하게 됩니다.

확인되지 않은 예외와 달리 확인된 예외는 반드시 `try-catch`나 메서드에서 `throws`를 선언해야합니다.

## 2. Getting info about an exception

`catch`에서 예외를 감지하면 예외의 정보를 얻을 수 있습니다.

`e.getMessge()`, `e.printStackTrace()`, `e.getCause()`로 예외 발생 정보를 얻을 수 있습니다.

## 3. Catching multiple exceptions

자바는 같은 `try` 블럭 내에 여러 핸들러의 사용을 지원합니다.

```java
try {
    // 예외를 던질 수 있는 코드
} catch (IOException e) {
    // IOException과 서브클래스 처리
} catch (Exception e) {
    // Exception과 서브클래스 처리
}
```

여러 `catch`블럭을 사용할 때 보다 특정한 핸들러를 먼저 작성해야합니다.(`IOException` -> `Exception`) 그렇지 않으면 코드는 컴파일 되지 않습니다.

**다중 catch문(자바 7)**

```java
try {
    // 예외를 던질 수 있는 코드
} catch (SQLException | IOException e) {
    // SQLException, IOException과 서브 클래스 처리
    System.out.println(e.getMessage());
} catch (Exception e) {
    // 그 외 예외 처리
    System.out.println("Something goes wrong");
}
```

다중 catch문의 대안들은 서로의 서브 클래스가 될 수 없습니다.

## 4. The finally block

**`finally`**
`try`블럭에서 예외가 발생하든 아니든 항상 실행되는 블럭

**`try-catch-finally`의 흐름**

```java
try {
    System.out.println("inside the try block");
    Integer.parseInt("101abc"); // NumberFormatException
} catch (Exception e) {
    System.out.println("inside the catch block");
} finally {
    System.out.println("inside the finally block");
}

System.out.println("after the try-catch-finally block");
```

결과:

```no-highlight
inside the try block
inside the catch block
inside the finally block
after the try-catch-finally block
```

위 예시에서 예외가 발생하지 않으면 catch 블럭의 구문이 제외됩니다.

**`try-finally` 템플릿**

```java
try {
    // 예외를 던질 수도 있는 코드
} finally {
    // 항상 실행되는 코드
}
```

## 5. Where to handle an exception

기술적으로 예외는 발생하는 메서드나 호출되는 메서드에서 처리할 수 있습니다.

**예외를 처리하는 가장 좋은 방법**
예외를 기반으로 올바른 결정을 내릴 수 있는 충분한 정보가 있는 메서드에서 처리하는 것입니다.
