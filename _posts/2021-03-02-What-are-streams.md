---
title: '스트림이란?'
excerpt: '스트림과 스트림의 종류, 작동 방식을 알아보자'

categories:
  - Java
tags:
  - Working with data
  - IO streams

toc: true
toc_label: 'Table of contents'
toc_sticky: true
---

> 출처 [What are streams - JetBrains Academy](https://hyperskill.org/learn/step/5533)를 보고 요약한 내용입니다.

**스트림**은 자바가 제공하는 추상화로 파일, 네트워크, 기타 리소스 같은 외부와 데이터를 송수신합니다.

## 1. Input and output streams

**IO 스트림**은 물의 흐름과 같이 시작 지점과 끝 지점이 있습니다.

**IO 스트림의 종류**

- **input stream** 데이터소스로 부터 데이터를 읽습니다.
- **output** **stream** 특정 데이터 소스에 데이터를 씁니다.

IO 스트림의 예로 **System.in** 과 **System.out**이 있습니다. 두 스트림은 콘솔과 데이터를 송수신합니다.

## 2. Byte and char streams

**스트림의 종류**

- **byte streams** 데이터를 바이트로 읽고 씁니다.
- **char streams** 데이터를 16-bit Unicode 형식에 따라 문자로 읽고 씁니다.

**Char streams**는 텍스트 데이터를 쉽게 처리할 수 있게 하고 **byte streams**는 char streams보다 낮은 수준이지만 multimedia를 포함한 모든 타입의 데이터를 처리할 수 있습니다.

## 3. Buffered streams

**Buffered streams**는 임시 메모리를 사용하는 스트림입니다. 이러한 스트림은 임시 메모리에서 데이터를 읽거나 쓴 다음 데이터 소스나 해당하는 프로그램으로 데이터를 이동합니다.

**Buffer**는 임시 메모리로 일반적으로 byte 또는 character 배열입니다.

**Buffering**은 buffer를 사용하여 데이터가 읽고 쓰여지는 과정입니다.

**Buffer를 쓰는 이유**는 몇몇 데이터 소스나 프로그램 사이에 데이터가 송수신하는데 상당한 시간 간격이 발생하기 때문입니다.
따라서 Buffering은 이들의 상호작용 횟수를 최소화하는 일종의 최적화라고 할 수 있습니다.

**Output streams에서 buffering이 작동하는 방식**

1. 스트림에 데이터를 쓴다.
2. buffer에 데이터를 쌓는다.
3. buffer에 데이터가 다 쌓이면 buffer에 쌓인 모든 데이터가 데이터 소스에 쓰여집니다.

**Input streams에서 buffering이 작동하는 방삭**

1. 스트림은 buffer가 다 찰 때 까지 데이터 소스로 부터 데이터를 읽습니다.
2. buffer의 데이터를 모두 읽을 때 까지 스트림은 buffer로부터 데이터를 프로그램으로 가져옵니다.
3. buffer의 데이터를 모두 읽으면 스트림은 처음과 같이 데이터 소스로 부터 데이터를 읽습니다.
