---
title: '생성자'
excerpt: '생성자와 this 키워드에 대해 알아보자'

categories:
  - Java
tags:
  - Code organization
  - OOP
  - Classes and objects
  - Classes and members

toc: true
toc_label: 'Table of contents'
toc_sticky: true
---

> 출처 [Constructor - JetBrains Academy](https://hyperskill.org/learn/step/3535)를 보고 요약한 내용입니다.

**생성자**는 클래스의 **새로운 객체**를 초기화하는 특별한 메서드입니다.

클래스의 생성자는 인스턴스가 `new` 키워드로 생성되어질 때 호출됩니다.

**생성자와 다른 메서드의 차이점**

- 생성자의 이름은 해당 클래스의 이름과 같습니다.
- 리턴 타입이 없습니다.(`void`도 포함합니다.)

**생성자의 역할**

- 클래스의 **인스턴스**(객체) 초기화
- 객체가 만들어질 때 필드에 값을 할당
- 파라미터로 주어진 값들로 필드들을 초기화

## 1. Using constructors

`Student`는 이름, 학년, 학번이라는 필드들을 가지고 있습니다.

```java
class Student {

    String name;
    int grade;
    long studentId;

    public Student(String name, int age, long studentId) {
        this.name = name;
        this.grade = grade;
        this.studentId = studentId;
    }
}
```

생성자를 사용하여 인스턴스들을 생성합니다.

```java
Patient student1 = new Student("Henry", 3, 20180304);
Patient student2 = new Student("Mark", 4, 20161233);
```

두 학생들은 같은 필드를 가지고 있지만, 이 필드들의 값들은 다릅니다.

## 2. Keyword this

`Student` 생성자는 세개의 파라미터들을 가지고 있습니다.

```java
this.name = name;
this.grade = grade;
this.studentId = studentId;
```

생성자의 코드 블럭에 있는 `this`는 필드들을 초기화하기 위해 사용됩니다.

**`this` 란?**

`this`는 현재 객체를 참조한다는 키워드로 위의 코드 같이 인스턴스의 변수의 이름이 생성자 또는 메서드 변수의 이름과 같을 때 사용됩니다.

**`this`를 사용하는 이유**

`this` 키워드 없이 `name = newName`과 같이 변수의 이름을 다르게 하여 구분할 수도 있지만 `name`과 `newName`은 같은 것을 가리키기 때문에 의미 상 좋지 않은 사례입니다. 또한 변수가 많아지면 코드는 불명확해지고 오버로드가 증가하게 됩니다.

## 3. Default and no-argument constructor

컴파일러는 생성자가 없는 클래스에 자동적으로 **인자가 없는 기본 생성자**를 제공합니다.

```java
class Student {

    String name;
    int grade;
    long studentId;
}
```

인자가 없는 기본 생성자를 사용하여 클래스의 인스턴스를 생성할 수 있습니다.

```java
Student student = new Student();
```

기본 생성자로 인스턴스를 생성하면 모든 필드들은 그들의 타입의 기본 값으로 초기화됩니다.

특정 생성자를 정의했다면 기본 생성자는 만들어지지 않습니다.

또한 아무 인자가 없는 생성자를 이용하여 클래스 필드의 기본값을 설정할 수 있습니다.

```java
class Student {

    String name;
    int grade;
    long studentId;

    public Patient() {
        this.name = "Unknown";
    }
}
```

필드의 기본 값이 `null`보다 나을 때 이러한 인자가 없는 생성자가 유용합니다.

## 4. To sum up

- 자바 클래스는 객체를 초기화하기 위한 생성자를 가지고 있습니다.
- 생성자는 해당하는 클래스의 이름과 같은 이름을 가지고 있습니다.
- 생성자는 `void`를 보함한 어떤 리턴 타입을 가지고 있지 않습니다.
- 만약 명시적인 생성자를 가지고 있지 않다면, 자바 컴파일러가 자동적으로 인자가 없는 기본 생성자를 제공합니다.
- 같은 것을 나타내는 새 변수를 도입하고 코드를 더 명확하게 만들고 추가 변수로로드를 줄이려면 키워드`this`가 사용됩니다.
