---
title: 'Try with resources'
excerpt: '외부 리소스 사용 시 어떤 일이 발생하는지, 어떻게 리소스를 닫는 것을 수행되는지, 이것이 왜 중요한지 알아보자'

categories:
  - Java
tags:
  - Errorless code
  - Exception handling

toc: true
toc_label: 'Table of contents'
toc_sticky: true
---

> 출처 [Try with resources - JetBrains Academy](https://hyperskill.org/learn/step/9734)를 보고 요약한 내용입니다.

## 1. Why close?

입력 스트림이 생성되면 JVM이 OS에게 파일을 사용한다고 알립니다. JVM 프로세스가 충분한 권한이 있고 문제가 없으면 OS는 **file descriptor**를 반환합니다. file descriptor은 프로세스가 파일에 접근할 때 사용하는 특수한 지시자입니다. 문제는 이 file descriptior의 수가 제한되었다는 것입니다. 따라서 파일에 대한 작업을 마쳤다는 것과 받은 file descriptor를 재사용을 위해 해제할 수 있다는 것을 OS에게 알려야합니다.

`close` 메서드가 이를 알리기 위해 사용되고 메서드가 호출되면 JVM은 스트림과 연관된 모든 시스템 자원을 해제합니다.

## 2. Pitfalls

`close` 메서드를 호출하여 자원을 해제할 수 있지만 이 메서드가 호출되지 않을 때가 있습니다.

```java
void main(String[] args) throws IOException {
    Reader reader = new FileReader("/Users/grand/file.txt");
    // 예외를 던지는 코드
    reader.close();
}
```

`close`가 호출되기 전 코드가 잘못되어 예외가 던져졌다면 시스템 자원은 해제되지 않습니다. 이 문제는 **try-catch-finally** 구조로 해결할 수 있습니다.

```java
void readFile() throws IOException {
    Reader reader = null;
    try {
        reader = new FileReader("/Users/grand/file.txt");
        // 예외를 던지는 코드
    } finally {
        reader.close();
    }
}
```

`/Users/ghYoon93/file.txt`가 존재한다고 가정하여 `finally`에서 `Reader`가 `null`인지 확인하지 않겠습니다. 하지만 실제 애플리케이션에서는 확인해줘야합니다.

위의 예시에서는 예외를 던지는 코드가 `close` 메서드 호출에 영향을 주지 않습니다.

**try-catch-finally 구조의 문제점**

`close` 메서드가 잠재적으로 예외를 발생시킬 수 있습니다.

**_try_** 블럭과 **_finally_** 블럭에 예외가 있다고 가정해보겠습니다.

```java
void readFile() throws IOException {
    Reader reader = null;
    try {
        reader = new FileReader("/Users/grand/file.txt");
        throw new RuntimeException("Exception1");
    } finally {
        reader.close(); // throws new RuntimeException("Exception2")
    }
}
```

두 예외가 던져지면 **_finally_** 블럭의 예외만 메서드 외부에 던져집니다. 즉, **_try_** 블럭에서 예외가 발생되었다는 것을 절대 알 수 없습니다.

**_try_**에서도 예외가 발생했다는 것을 알고 싶다면 다음과 같이 `reader.close()`에 대한 예외 처리를 해줘야합니다.

```java
void readFile() throws IOException {
    Reader reader = null;
    try {
        reader = new FileReader("/Users/grand/file.txt");
        throw new RuntimeException("Exception1");
    } finally {
		try {
            reader.close(); // throws new RuntimeException("Exception2")
        } catch (Exception e) {
            // Exception2 처리
        }
    }
}
```

이제 **_try_**블럭의 `Exception1` 예외도 외부에 던져집니다. 하지만 여전히 두 예외에 대한 정보를 저장하지 않으며 이 정보를 잃고 싶지 않을 수 있습니다. 이제 이러한 상황을 잘 처리하는 방법을 알아보겠습니다.

## 3. Solution

**try-with-resources**

간단하고 안전한 방법으로 자바 7에서 소개되었습니다.

```java
void readFile() throws IOException() {
    try (Reader reader = new FileReader("/Users/ghYoon93/file.txt")) {
        // 코드 작성
    }
}
```

**try-with-resources**의 소괄호(`( )`)에는 입력 스트림 인스턴스 생성 구문을 포함하고 있습니다. 입력 스트림 인스턴스는 다음과 같이 여러개를 생성할 수 있습니다.

```java
try (Reader reader1 = new FileReader("/Users/ghYoon93/file1.txt");
    Reader reader2 = new FileReader("/Users/ghYoon93/file2.txt")) {

}
```

중괄호(`{ }`)는 소괄호에서 생성된 객체를 다루는 코드를 포함하고 있습니다.

보다시피 `close` 메서드의 명시적인 호출이 없습니다. 소괄호에서 선언된 모든 객체들의 `close`메서드는 암묵적으로 호출됩니다.

자바 9부터는 입력 스트림을 구조 밖에서 초기화하고 소괄호에서 선언할 수 있습니다.

```java
Reader reader = new FileReader("/Users/ghYoon93/file.txt");
try (reader) {
    // 코드 작성
}
```

다음과 같이 try-catch-finally의 일부로 try-with-resources를 같이 사용할 수 있습니다.

```java
try (Reader reader = new  FileReader("Users/ghYoon93/file.txt")) {
    // 코드 작성
} catch (IOException e) {
    ...
} finally {
    ...
}
```

**_try_** 블럭과 `close` 메서드가 `Exception1`과 `Exception2` 예외를 던지는 경우

```java
void readFile() throws IOException {
    try (Reader reader = new FileReader("/Users/ghYoon93/file.txt")) {
        throw new RuntimeException("Exception1");
    }
}
```

두 예외가 발생한다고 가정하면 다음과 같은 예외 정보가 출력됩니다.

```
Exception in thread "main" java.lang.RuntimeException: Exception1
    at ...
    Suppressed: java.lang.RuntimeException: Exception2
        at ...
```

## 4. Closeable resources

파일 기반의 리소스 이외에도 웹 혹은데이터베이스 연결 등 리소스를 해제해야하는 외부 리소스들이 있습니다. 이러한 리소스를 다루는 클래스는 `close`메서드를 가지고 있고 그러므로 try-with-resources 구문으로 감쌀 수 있습니다.

## 5. Conclusion

부적절한 자원 핸들링은 심각한 문제들을 일으킬 수도 있습니다.

파일, 웹, 데이터베이스 등의 외부 소스와 관련된 리소스는 사용 후 해제해야합니다.

외부 소스들을 다루는 표준 라이브러리 클래스들은 리소스 해제를 위한 `close` 메서드를가지고 있습니다.

try-with-resources는 리소스 해제를 간단하게 하기 위한 구조입니다.
