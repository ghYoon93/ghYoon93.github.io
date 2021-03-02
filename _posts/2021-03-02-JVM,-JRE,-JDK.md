---
title: 'JVM, JRE, JDK'
excerpt: 'JVM, JRE, JDK를 알아보자'

categories:
  - Java
tags:
  - Java internals
  - JVM
  - JRE
  - JDK

toc: true
toc_label: 'Table of contents'
toc_sticky: true
---

> 출처 [JVM, JRE, and JDK - JetBrains Academy](https://hyperskill.org/learn/step/3499)를 보고 요약한 내용입니다.

## 1. Java Virtual Machine (JVM)

**JVM**은 물리적 컴퓨터의 가상 시뮬레이션으로 자바 바이트코드 클래스 파일들을 실행합니다.
JVM은 많은 하드웨어와 소프트웨어 플랫폼에서 사용할 수 있으므로, Java 바이트코드는 거의 모든 곳에서 실행할 수 있습니다. 따라서 자바 바이트코드로 컴파일된 프로그램은 **플랫폼에 독립적**입니다.

## 2. Java Runtime Environment (JRE)

**JRE**는 컴파일 된 JVM 프로그램들을 실행하기 위한 실행 환경입니다. JRE는 JVM과 Java Class Library(**JCL**)을 포함하고 있습니다.

**JCL**은 collections, input/out 등 많은 자바 라이브러리로 구성되어있습니다.

**JRE** is an execution environment for **running** compiled JVM programs. JRE includes Java Virtual Machine (**JVM**) and Java Class Library (**JCL**).

**JCL** consists of many libraries including input/output, collections, security, classes for parsing XML, user interface toolkits, and many others. Your program can use these libraries.

컴파일된 프로그램을 JRE에서 실행할 때, JVM은 작성한 프로그램과 JCL의 바이트코드 클래스파일을 사용합니다.

## 3. Java Development Kit (JDK)

**JDK**는 자바 플랫폼의 프로그램들을 개발하기 위한 패키지입니다. JDK는 JRE와 Java 컴파일러, 디버거, 아카이버 등의 개발자를위한 툴들을 포함하고 있습니다.

자바 컴파일러는 **\*.java**를 **\*.class**로 변환하며 아카이버로 몇몇의 **\*.class** 파일들을 하나의 Java Archive**(JAR-file)**로 압축할 수 있습니다.

```
Java 11부터는 대부분의 JVM에서 JRE를 별도로 다운로드할 수 없습니다.
JVM 11 이상에서 프로그램을 실행시키기 위해 JDK를 필수로 설치해야합니다.
```
