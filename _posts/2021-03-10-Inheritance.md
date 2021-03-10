---
title: '상속'
excerpt: '상속에 대해 알아보자'

categories:
  - Java
tags:
  - Code organization
  - OOP
  - Class hierarchies
  - Building class hierarchies

toc: true
toc_label: 'Table of contents'
toc_sticky: true


---

> 출처 [Inheritance - JetBrains Academy](https://hyperskill.org/learn/step/3583)를 보고 요약한 내용입니다.

## 이전 포스트

* [생성자](https://ghyoon93.github.io/java/Constructor/)
* [getter/setter](https://ghyoon93.github.io/java/Getters-and-setters/)

## 1. Extending classes

**상속**은 다른 클래스(기본 클래스)에서 새 클래스를 파생시키는 메커니즘으로 OOP의 주요 원칙 중 하나입니다.

상속을 받은 클래스는 기본 클래스의 일부 필드와 메서드를 사용할 수 있습니다. 이러한 특징으로 편리한 클래스 계층 구조를 구축하고 기존 코드를 재사용할 수 있습니다.

**상속과 관련된 용어**

* **하위 클래스(파생 클래스, 확장 클래스, 자식 클래스)** - 다른 클래스에서 파생된 클래스
* **슈퍼 클래스(기본 클래스, 상위 클래스, 부모 클래스)** - 서브 클래스가 파생된 클래스

**상속의 기본 문법**

```java
class SuperClass {}
class SubClassA extends SuperClass {}
class SubClassB extends SuperClass {}
class SubClassC extends SubClassA {}
```

**자바에서의 상속 주요 특징**

* 자바는 여러개의 클래스 상속을 지원하지 않습니다. 즉, 클래스는 한개의 슈퍼 클래스만 상속받을 수 있습니다.
* 클래스는 여러 계층을 가질 수 있습니다. (`C`클래스는 `A`클래스로부터 확장된 `B`클래스로부터 확장될 수 있습니다.)
* 슈퍼클래스는 여러개의 서브클래스를 가질 수 있습니다.

**서브클래스**

* 슈퍼클래스의 public과 protected 멤버를 상속 받습니다.
* 상속 받은 멤버 외에도 새로운 멤버를 추가할 수 있습니다.
* 상속 받은 멤버와 새로운 멤버는 같은 방법으로 사용됩니다.

**슈퍼클래스의 생성자**

슈퍼클래스의 생성자는 상속되지 않습니다. 하지만 `super` 키워드로 서브클래스에서 슈퍼클래스의 생성자를 호출할 수 있습니다.

**IS-A 관계**

**상속**은 흔히 **IS-A** 관계라고 표현됩니다. 슈퍼클래스는 일반적인 것을 표현하며 서브클래스는 특정하거나 부분적인 것을 표현합니다.

## 2. An example of a class hierarchy

클라이언트에게 서비스를제공하는 통신회사를 예로 들겠습니다. 통신회사는 매니저와 프로그래머로만 구성되어있습니다.

클라이언트을 포함한 이 회사의 활동과 관련된 사람들에 대한 클래스 계층 구조를 고려해 봅시다.

* `Person`은 기본클래스로 이름, 출생연도, 주소를 가지고 있습니다.
* `Client`는 계약 번호와 회원 등급을 가지고 있습니다.
* `Employee`는 입사일과 월급을 가지고 있습니다.
* `Programmer`는 사용할 수 있는 프로그래밍 언어들을 가지고 있습니다.
* `Manager`는 환한 미소를 가지고 있습니다.

```java
class Person {
    protected String name;
    protected int yearOfBirth;
    protected String address;

    // getter와 setter 생략
}

class Client extends Person {
    protected String contractNumber;
    protected boolean gold;

    // getter와 setter 생략
}

class Employee extends Person {
    protected Date startDate;
    protected Long salary;

    // getter와 setter 생략
}

class Programmer extends Employee {
    protected String[] programmingLanguages;

    public String[] getProgrammingLanguages() {
        return programmingLanguages;
    }

    public void setProgrammingLanguages(String[] programmingLanguages) {
        this.programmingLanguages = programmingLanguages;
    }
}

class Manager extends Employee {
    protected boolean smile;

    public boolean isSmile() {
        return smile;
    }

    public void setSmile(boolean smile) {
        this.smile = smile;
    }
}
```

계층은 전체적으로 두개의 레벨과 5개의 클래스로 이루어져있습니다. 모든 필드는 `protected`로 서브클래스에서 볼 수 있습니다. 각 클래스는 getter와 setter를 가지고 있습니다.

`Programmer`객체를 만들어 상속 받은 setter를 이용해 상속 받은 필드를 채워봅시다. 필드값은 상속 받은 getter로 읽을 수 있습니다.

```java
Programmer p = new Programmer();

p.setName("John Elephant");
p.setYearOfBirth(1985);
p.setAddress("Some street, 15");
p.setStartDate(new Date());
p.setSalary(500_000L);
p.setProgrammingLanguages(new String[] { "Java", "Scala", "Kotlin" });

System.out.println(p.getName()); // John Elephant
System.out.println(p.getSalary()); // 500000
System.out.println(Arrays.toString(p.getProgrammingLanguages())); // [Java, Scala, Kotlin]
```

`Programmer` 외에도 계층 내 모든 클래스의 인스턴스를 만들 수 있습니다.

상속은 코드의 재사용 및 편리한 계층 설계를 위한 강력한 메커니즘을 제공합니다. 현실에서의 많은 것들도 일반적인 개념에서 더 특정한 개념에 이르는 계층 구조를 만들 수 있습니다.

## 3. Final classes

`final` 키워드로 선언된 클래스는 서브클래스를 가질 수 없습니다. 표준 클래스에서는 `Integer`, `Long`, `String`, `Math`클래스가 `final`로 선언되었습니다.

## 4. Conclusion

**상속**을 사용하면 하위클래스(자식)가 상위 클래스(부모)의 일부 멤버를 사용할 때 클래스를 계층적으로 구축할 수 있습니다. 

이러한 계층은 여러 레벨을 가질 수 있지만 모든 클래스는 하나의 슈퍼 클래스만 상속 받을 수 있습니다. 

좋은 클래스 계층은 코드 중복을 방지하고 프로그램을 모듈화하는 데 도움이 됩니다. 

클래스에 서브 클래스가 없어야하는 경우 `final`로 선언해야합니다.