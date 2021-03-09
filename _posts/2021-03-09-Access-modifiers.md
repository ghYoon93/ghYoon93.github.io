---
title: '접근 제어자'
excerpt: '접근 제어자의 종류와 허용 범위를 알아보자'

categories:
  - Java
tags:
  - Code organization
  - OOP
  - Classes and objects
  - Access control

toc: true
toc_label: 'Table of contents'
toc_sticky: true
---

> 출처 [Access modifiers - JetBrains Academy](https://hyperskill.org/learn/step/3589)를 보고 요약한 내용입니다.

## 이전 포스트

- [생성자](https://ghyoon93.github.io/java/Constructor)
- 인스턴스 메서드

- [패키지](https://ghyoon93.github.io/java/Package)

## 0. Access modifiers

**접근 제어자**

코드 또는 코드의 특정 부분을 사용할 수 있는 사람을 지정하는 키워드입니다. 접근 제어자는 필드, 메서드 또는 모든 클래스 앞에 위치할 수 있습니다.

**접근 제어자의 종류**

- `public`
- `package-private`
- `protected`
- `private`

## 1. OK, so why should I use them?

**접근 제어자를 사용해야 하는 이유**

- 명확한 코드
- 안전한 코드

**명확한 코드**

접근 제어자는 코드의 복잡한 동작 메서드에 접근을 제한하고 코드의 기능을 사용할 수 있는 메서드를 공개함으로써 사용자에게 명확한 코드를 제공합니다.

**안전한 코드**

접근 제어자는 코드의 중요한 부분을 보호하여 다른 사용자가 코드의 동작 방식을 수정할 수 없도록 보장합니다. 따라서 코드는 개발자가 원래 의도했던 방식으로 동작하고 결과를 반환할 수 있습니다.

## 2. Public and package-private classes

코드의 접근은 top-level 항목과 low-level 항목으로 나눌 수 있습니다.

**top-level 항목**
클래스의 외부에서 명시적으로 사용되는 필드와 메서드입니다.

**low-level 항목**
클래스 내부에서 사용되는 필드와 메서드입니다.

top-level과 low-level 항목은 클래스에도 적용할 수 있습니다.

**low-level 항목이 필요한 이유**
low-level 항목을 사용하면 코드를 구조화하고 분해하기 위해 top-level 항목(클래스, 메서드, 필드)의 책임을 더는 데 도움이 됩니다.

**top-level 클래스가 가질 수 있는 제어자**

- package-private(기본, 명시적 제어자 없음)

* public

**package-private**
package-private 클래스는 클래스 선언 시 제어자가 없습니다. package-private 클래스는 같은 패키지에서만 보여지고 사용할 수 있습니다.

**public**
public 클래스는 클래스 선언 시 public 제어자를 붙입니다. public 클래스는 모든 곳에서 보여지고 사용할 수 있습니다.

사용자를 위한 메서드를 포함한 클래스는 **public** 접근을 허용하며 low-level 로직 메서드들이 있는 클래스는 **package-private** 접근을 허용합니다.

**Getter and setter**
getter와 setter 메서드는 클래스를 생성 시 데이터를 보호하고 숨기기 위해 사용됩니다. getter 메서드는 필드 값을 반환하는 반면 setter 메서드는 데이터 값을 설정하거나 변경합니다.

다른 패키지의 클래스에서 사용 또는변경할 수 없게 하려면 public 제어자를 사용하면 안됩니다.

## 3. Private members

**클래스의 멤버(필드 또는 메서드)에서 선택할 수 있는 접근 제어자**

- private
- package-private
- protected
- public

**필드의 private 접근 제어자**
필드는 보통 다른 클래스에서 접근하지 못하도록 **private**으로 선언합니다. 필드는 클래스 내부에서만 사용되며 다른 클래스에서 직접 접근하거나 변경할 수 없습니다. 필드값에 접근하거나 값을 변경하는 것을 허용한다면 getter와 setter 같은 메서드를 제공해야합니다.

**메서드의 private 접근 제어자**
prviate 메서드는 내부 low-level의 로직 구현을 숨겨 public 메서드를 더 간단하고 가독성있게 만드는 데 사용됩니다.

**private 멤버의 사용 예**

```java
public class Counter {
    private long current = 0;

    public long getCurrent() {
        return current;
    }

    public long inc(){
        inc(1L);
        return current;
    }

    private void inc(long val) {
        current += val;
    }
}
```

`current`는 private 필드이기 때문에 **getter 메서드**인 `getCurrent()`메서드로만 읽고 `inc()`메서드로 변경할 수 있습니다. 마지막 `inc()` 메서드는 단순히 `current`의 값을 증가시키기 때문에 **setter 메서드**라고 보기 어렵습니다.

**생성자의 private 접근 제어자**
private 클래스 생성자는 클래스의 내부에서만 생성자가 사용될 떄 필요로 합니다.

## 4. Package-private members

**package-private** 접근 제어자는 키워드를 필요로 하지 않습니다. 아무 접근자가 없는 필드, 메서드, 생성자는 같은 패키지 내의 클래스에서 읽고 변경할 수 있습니다.

## 5. Protected and public members

클래스 멤버가 **protected** 접근 제어자를 가지고 있으면 이 클래스 멤버는 같은 패키지 안의 클래스와 이 클래스의 모든 하위 클래스(다른 패키지의 하위 클래스 포함)에서 접근할 수 있습니다.

**public** 접근 제어자는 필드, 메서드, 클래스를 사용하는데 있어서 제한이 없다는 의미입니다.

**public 접근 제어자 사용 용도**

- 생성자
- class API 제공 메서드

## 6. Summary

**접근 제어자 별 접근 가능 범위**

- **private** - 클래스 내부
- **package-private (기본, 암묵적)** - 같은 패키지 내 모든 클래스
- **protected** - 같은 패키지 내 클래스와 하위 클래스
- **public** - 모든 클래스

클래스 멤버는 적합한 수준의 접근 제어자를 가져야합니다.
