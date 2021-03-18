---
title: 'Input streams'
excerpt: '자바 프로그램에서 외부 데이터를 사용하는 메커니즘을 알아보자'

categories:
  - Java
tags:
  - Working with data
  - IO streams

toc: true
toc_label: 'Table of contents'
toc_sticky: true


---

> 출처 [Input streams - JetBrains Academy](https://hyperskill.org/learn/step/9723)를 보고 요약한 내용입니다.

**input streams**
자바에서 데이터를 사용하는 공통적인 메커니즘

## 1. Sources

**sources**는 프로그램에 의해 사용되어지고 처리되는 모든 데이터입니다. input streams에서는 표준 입출력, 파일, 네트워크 연결, 메모리 내 버퍼, 객체가 sources가 됩니다.

특정 소스 사용의 구현은 매우 특정적이므로 각 소스에는 특정한 클래스가 필요합니다.

## 2. Character streams

Character input streams는 `char` 또는 `String` 같은 텍스트 데이터를 읽을 수 있습니다.

**텍스트를 읽기위한 클래스**

* `FileReader`
* `CharArrayReader`
* `StringReader`
* etc

> 이러한 클래스는 이름에 입력으로 사용하는 타입을 포함하며 `java.io.Reader`를 상속받기 때문에 보통 이름을 Reader로 마칩니다.

**Character streams의 공통 메서드**

* `int read()` 단일 문자를 읽습니다. 스트림의 끝에 도달하면 메서드는 `-1`을 반환합니다. 그 외에는 인코딩에 따라 문자를 숫자로 표현합니다. (아스키 코드)
* `int read(char[] cbuf)` 인자로 전달한 배열 크기만큼 문자들의 시퀀스를 할당합니다. 그리고 실제로 읽은 문자 수를 반환합니다. `int read()`와 마찬가지로 읽은 데이터가 없을 때에는 `-1`을 반환합니다.
* `int read(char[] cbuf, int off, int len)` 인자로 전달한 배열의 `off`인덱스 부터 `len`만큼의 문자들의 시퀀스를 할당합니다. 그리고 실제로 읽은 문자 수를 반환합니다.

위의 메서드는 입력이 가능하고 스트림의 끝에 도달할 때까지 프로그램의 실행을 막습니다. 또한 스트림을 사용한 뒤 `void close()`메서드를 호출하여 자원의 누수를 막아야합니다.

> `void close()` 메서드 대신 **try-with-resources** 구성으로 자원의 누수를 막을 수 있습니다.

## 3. Example of a character stream

`FileReader`를 예시로 들어 `Reader`클래스를 설명하겠습니다.

**`FileReader`의 생성자**

* `new FileReader(String fileName)`
* `new FileReader(String fileName, Charset charset)`
* `new FileReader(File file)`
* `new FileReader(File file, Charset charset)`

경로 문자열 혹은 `File` 객체로 파일에서 텍스트 데이터를 읽을 수 있습니다.

**`Charset`**

바이트 시퀀스에서 문자로 인코딩하는 방식을 선언하는 클래스입니다. 기본적으로 자바는 대부분의 작업에 적합한 **UTF-16** 인코딩 방식을 사용합니다. 파일이 다른 인코딩 방식을 가질 수 있으므로 파일에 적합한 charset을 사용해야 합니다.

**input stream이 쓰여진 `file.txt` 읽기**

```java
import java.io.*;

public class InputStreams {
    public static void main(String[] args) throws IOException {
        Reader reader = new FileReader("/Users/grand/file.txt");

        char first = (char) reader.read(); // i
        char second = (char) reader.read(); // n

        char[] others = new char[12];
        int number = reader.read(others); // 10

    }
}

```

코드를 실행하면  `others`는 `['p', 'u', 't', ' ', 's', 't', 'r', 'e', 'a', 'm', '\u0000', '\u0000']`를 포함하게 됩니다.

`\u0000`는 문자 타입의 배열의 기본값으로 기술적으로는 존재하지만 출력 시 아무 것도 표시되지 않습니다.

**텍스트 데이터 스트림을 스트림이 끝날 때 까지 하나  하나씩 읽기**

```java
package org.hyperskill.java.workingwithdata.IOstreams;

import java.io.*;

public class InputStreams {
    public static void main(String[] args) throws IOException {
        Reader reader = new FileReader("/Users/grand/file.txt");
        
        int charAsNumber = reader.read();
        while(charAsNumber != -1) {
            char character = (char) charAsNumber;
            System.out.println(character);
            charAsNumber = reader.read();
        }
        reader.close();

    }
}

```

## 4. Byte streams

표준 입력으로 부터 텍스트 데이터를 읽을 때 자주 사용하는 `Scanner`의 인스턴스를 만들  때 `System.in`을 생성자의 인자로 전달합니다.

```java
Scanner scanner = new Scanner(System.in);
```

`System.in`은 byte 입력 스트림 중 하나입니다.

**byte input  stream 클래스**

- `ByteArrayInputStream` `byte[]`를 읽을 때 사용합니다.
- `FileInputStream`  파일을 읽을 때 사용합니다.
- `AudioInputStream` 오디오 입력을 사용할 때 쓰는 방법입니다.
- etc

> 위의 클래스들은 `java.io.InputStream`을 상속 받기 때문에 입력으로 사용하는 데이터 타입과 InputStream으로 클래스의 이름을 짓습니다.

**byte streams의 공통 메서드**

* `abstract int read()` 단일 바이트를 읽습니다.
* `int read(byte[] b)` 바이트들을 읽고 바이트 배열에  할당합니다.
* `byte[] readAllBytes()` 모든 바이트를 읽습니다.

위의 메서드는 Charater input stream과 마찬가지로 읽은 바이트 수를 반환하고 읽은 바이트가 없을 때 `-1`을 반환합니다.

각 입력 스트림 클래스는 `void close()` 메서드를 가지고 있어 시스템 자원을 해제할 수 있습니다.

## 5. Example of a byte stream

위의 예시에서 사용한 `file.txt`를 `FileInputStream` 클래스를 사용하여 읽어 보겠습니다.

**`FileInputStream`의 생성자**

- `new FileInputStream(String pathToFile)`
- `new FileInputStream(File file)`

**byte stream이 쓰여진 `file.txt` 읽기**

```java
import java.io.FileInputStream;

public class ByteStreamEx01 {
    public static void main(String[] args) {
        try (FileInputStream inputStream = new FileInputStream("/Users/grand/file.txt")) {
            byte[] bytes = new byte[5];
            int numberOfBytes = inputStream.read(bytes);
            System.out.println(numberOfBytes); // 5
            for (byte b : bytes) {
                System.out.print((char) b + " ");
            }
        } catch (Exception e) {

        }
    }
}
```

## 6. Testing input streams

`Scanner`가 아닌 다른 입력 스트림 클래스는 표준 입력을 받을 때 **Enter**를 **입력의 끝**이 아닌 줄 바꿈 기호(`\n`)로 문자들의 시퀀스에 포함됩니다. 따라서 입력 스트림은 계속해서 입력을 기다리게 됩니다.

입력 스트림을  닫기 위해서 ***end-of-file*** 이벤트를 제공해야합니다. IDEA에서의 ***end-of-file*** 이벤트는 **Ctrl + D** 혹은 **\<command>  + D** 입니다.

## 7. What type of stream should I use?

바이트와 문자 스트림의 가장 큰 차이점은 바이트 스트림은 입력된 데이터를 **bytes**로 읽고 문자 스트림은  입력 데이터를 문자들로 읽는 것 입니다.

텍스트를  읽어야한다면 문자 입력 스트림을 사용하고 그 외 (오디오, 비디오, zip 등)을 읽을 때는 바이트 입력 스트림을 사용합니다.

## 8. Conclusion

입력 스트림은 소스로부터 데이터를 읽을 때 문자 입력 스트림과 바이트 입력 스트림을 제공합니다.

**소스**
콘솔 또는 표준 입력, 파일, 문자열, 네트워크 연결 같은 데이터를  제공합니다.

**소스의 타입**

* byte
* character

**character input streams**
텍스트를 읽을 때만 사용, 보통 **데이터 타입 + Reader**

**byte input streams**
원시적인 바이트의 시퀀스를 읽을 때  사용, **데이터 타입 + InputStream**

