---
title: 'getter/setter'
excerpt: 'getter와 setter 메서드를 알아보자'

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

> 출처 [Getters and setters - JetBrains Academy](https://hyperskill.org/learn/step/3599)를 보고 요약한 내용입니다.

## 이전 포스트

- [접근 제어자](https://ghyoon93.github.io/java/Access-modifiers/)

## 1. Data encapsulation

데이터 캡슐화 원칙에 따라 클래스의 필드는 다른 클래스에서 직접적으로 접근하지 못하도록 숨겨져있습니다. 필드는 특정 클래스의 메서드를 통해서만 접근할 수 있습니다.

**Getters와 setters**는 숨겨진 필드에 접근할 수 있게 하는 특별한 타입의 메서드입니다.

**Getter**는 필드를 읽을 수만 있으며, **setter**는 필드를 작성 또는 수정만 할 수 있습니다. 두 메서드 모두 `public`이어야 합니다.

**Getters와 setters 사용 시 이점**

- 클래스의 필드를 읽기 전용, 쓰기 전용 또는 둘 다로 만들 수 있습니다.
- 클래스는 필드에 저장되는 값을 완전히 제어할 수 있습니다.
- 클래스의 사용자는 클래스의 데이터 저장 방법을 알 수 없으며 필드에 의존하지 않을 수 있습니다.

## 2. Getters and setters

자바는 getter와 setter 메서드에 대한 특별한 키워드를 제공하지 않습니다. 다만 메서드 이름으로 다른 메서드와 차이를 둘 수 있습니다.

**[JavaBeans Convention](https://docstore.mik.ua/orelly/java-ent/jnut/ch06_02.htm)에 따른 getter와 setter 메서드 작명 관례**

- **getters** 메서드의 이름은 **get**으로 시작하며 그 뒤에는 첫 문자가 대문자인 변수 이름이 따라옵니다.
- **setters** 메서드의 이름은 **set**으로 시작하며 그 뒤에는 첫 문자가 대문자인 변수 이름이 따라옵니다.

다만 `boolean` 타입의 변수는 **getter** 메서드가 **get** 대신에 **is**로 시작합니다.

**`Account` 클래스 예시**

```java
class Account {
    // private 필드 (직접 접근 불가)
    private long id;
    private String code;
    private long balance;
    private boolean enabled;
    private

    // public getters와 setters (필드 접근 메서드)
    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }

    public long getBalance() {
        return balance;
    }

    public void setBalance(long balance) {
        this.balance = balance;
    }

    // boolean 타입의 getter
    public boolean isEnabled() {
        return enabled;
    }

    public void setEnabled(boolean enabled) {
        this.enabled = enabled;
    }
}
```

**`Account` 클래스 사용 예시**

```java
class AccountApp {
    public static void main(String[] args) {
        Account account = new Account();
        account.setId(101001);
        account.setCode("9328409");
        account.setBalance(500_000);
        account.setEnabled(true);

		System.out.println(account.getId());      // 101001
		System.out.println(account.getCode());    // 9328409
		System.out.println(account.getBalance()); // 500000
		System.out.println(account.isEnabled());  // true
    }
}
```

**참조 타입 필드 주의 사항**

참조 타입의 필드는 메모리 주소를 공유하기 때문에 getter로 객체의 참조 타입 필드를 직접 반환하거나 setter로 참조 타입의 파라미터를 직접 할당하면 외부 클래스에 필드가 노출됩니다. 따라서 참조 타입의 필드를 호출 시 필드의 복제를 반환하고 값을 변경 시 파라미터의 복제를 객체의 필드에 할당하는 것이 좋습니다.

경우에 따라 **getter** 또는 **setter**는 더 정교한 로직을 가질 수 있습니다. 예를 들어 **getter**는 저장되지 않은 값(실행 시 계산)을 반환하거나 **setter**는 변경에 따라 다른 필드의 값을 수정하는 경우가 있습니다. 다만 이러한 로직은 꼭 필요한 경우에만 가지며 일반적으로는 최소한의 로직만 가져야합니다.

```java
class Patient {

    private String name;

    public Patient(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }

    // 파라미터의 값이 null이면 현재 값을 유지합니다.
    public void setName(String name) {
        if (name != null) {
            this.name = name;
        }
    }
}
```

## 3. Conclusion

외부 코드에서 필드의 접근을 제한하려면 필드를 `private`으로 만들고 필요한 필드만 **읽고 변경**하는 데 적합한 **getter 또는 setter** 메서드를 만듭니다. 메서드를 만들 때 명명 관례를 지켜야합니다.

## 다음 포스트

- 상속
