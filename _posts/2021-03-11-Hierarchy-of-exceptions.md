---
title: '예외의 계층'
excerpt: '예외의 계층과 기본 클래스에 대해 알아보자'

categories:
  - Java
tags:
  - Errorless code
  - Exception handling

toc: true
toc_label: 'Table of contents'
toc_sticky: true

---

> 출처 [Hierarchy of exceptions - JetBrains Academy](https://hyperskill.org/learn/step/3570)를 보고 요약한 내용입니다.

자바는 주로 객체  지향 언어입니다. 이러한 패러다임에서 모든 예외는 클래스 계층으로 이루어진 특별한 클래스의 객체로 간주됩니다.

## 1. Hierarchy of exceptions

**Throwable**

`java.lang.Throwable`은 모든 예외의 기본 클래스입니다.

**Throwable이 제공하는 메서드**

* `String getMessage()` 예외 객체의 자세한 문자열 메세지를 반환
* `Throwable getCause()`  예외 또는 `null`의 원인을 반환
* `printStackTrace()` 기본 에러 스트림의 스택 트레이스를 출력

**`Throwable`의 자식 클래스**

* `java.lang.Error`
* `java.lang.Exception`

**`java.lang.Error`**

`Error`의 서브 클래스는 JVM 의 low-level 예외(`OutOfMemoryError`, `StackOverflowError`)를 나타냅니다.

**`java.lang.Exception`**

`Exception`의 서브 클래스는 예외적인 상황(`RuntimeException`, `IOException`)을 다룹니다.

**`RuntimeException`**

`Exception`의 특수한 서브 클래스입니다. 소위 **확인되지 않은** 예외(`ArithmeticException`, `NumberFormatException`, `NullPointerException`)를 나타냅니다.

예외의 기본 클래스(`Throwable`,`Exception`,`RuntimeException`, `Error`)는 `java.lang` 패키지에 있습니다. 이 클래스들은 가져올 필요가없습니다. 그러나 이들의 서브 클래스는 다른 패키지에 있을 수도 있습니다.

## 2. Checked and unchecked exceptions

모든 예외는 확인된 예외와 확인되지 않은 예외로 나눌 수 있습니다. 두 예외는 기능적으로 동일하지만 컴파일 시점에서는 차이점이 있습니다.

**Checked exceptions**

`RuntimeException` 서브 클래스를 제외한 `Exception` 클래스에 의해 표현됩니다. 컴파일러는 프로그래머가 프로그램의 예외를 예상했는 지 확인합니다.

예상되는 예외를 가지고 있는 메서드는 `throws` 키워드를 사용해서 선언되어야합니다.

**Checked exceptions 예시**

```java
public static String readLineFromFile() throws FileNotFoundException {
    Scanner scanner = new Scanner(new File("file.txt")); // java.io.FileNotFoundException
    return scanner.nextLine();
}
    
```

`FileNotFoundException`은 기본적인 checked exception입니다. `Scanner` 생성자는 특정 파일이 없다고 예상할 수 있기 때문에 `FileNotFoundException` 예외를 선언합니다. 예외를 발생시키는 코드가 있기 때문에 메서드 선언 시 `throws`를 포함해야 합니다.

**Unchecked exceptions**

`RuntimeException` 클래스와 이것의 모든 서브클래스에 의해 표현됩니다. 컴파일러는 프로그램에서 이러한 예외 발생을 예상했는 지 확인하지 않습니다.

`Error`클래스와 하위 클래스도 unchecked exceptions로 간주됩니다. 하지만 이것들은 분리된 클래스를 형성합니다.