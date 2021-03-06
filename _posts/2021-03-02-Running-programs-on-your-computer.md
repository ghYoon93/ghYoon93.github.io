---
title: '컴퓨터에서 프로그램 실행시키기'
excerpt: '터미널에서 프로그램을 실행시키는 방법을 알아보자'

categories:
  - Java
tags:
  - Java internals
  - Inside the JVM

toc: true
toc_label: 'Table of contents'
toc_sticky: true
---

> 출처 [Running programs on your computer - JetBrains Academy](https://hyperskill.org/learn/step/3746)를 보고 요약한 내용입니다.

## 1. Installing Java on your computer

자바 프로그램을 실행하기 전에 **JDK**를 먼저 설치해주세요.

JDK를 설치하고 터미널에 다음과 같은 명령어를 입력합니다.

```java
java -version
```

제대로 설치되었다면 다음과 같이 JDK의 버전이 터미널에 출력됩니다.

![자바 버전](/assets/images/version-check-result.png)

만약 이러한 결과가 나오지 않았다면 운영 체제의 `path`에 환경 변수를 설정했는지 확인해보세요.

## 2. Writing a program

**자바 프로그램 작성하기**

아무 텍스트 에디터를 사용하여 `Main.java` 파일을 생성하고 다음 코드를 작성 후 저장합니다.

```java
public class Main {

    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

## 3. Compiling and running a program

프로그램을 작성했다면 자바 컴파일러로 `Main.java` 파일을 컴파일해야합니다.

터미널을 열어 위치를 `Main.java`가 있는 디렉토리로 이동한 후 다음과 같은 명령어를 입력합니다.

```java
javac Main.java
```

`javac` 명령어는 컴파일러에게 소스코드를 바이트코드로 변환하도록 요청합니다. 컴파일이 되었으면 같은 디렉토리에 `Main.class`가 생성됩니다.

이제 다음 명령어를 입력하여 컴파일 된 프로그램을 실행합니다.

```java
java -cp . Main
```

`java` 명령어는 자바 어플리케이션을 실행합니다. JRE를 시작하고 `Main` 클래스의 main 메서드를 호출하여 이를 실행합니다.

`-cp` 옵션은 유저가 정의한 클래스와 패키지의 위치를 지정합니다. `.`은 현재 터미널의 위치를 의미합니다.

프로그램이 제대로 실행되었다면 다음과 같이 터미널에 결과가 출력됩니다.

```java
Hello, Java
```

Java 11부터는 `java Main.java`명령어만으로 자바 소스 파일을 컴파일하고 실행할 수 있습니다. 메모리 상에 파일을 컴파일하여 `.class`파일이 생성되지 않습니다.
