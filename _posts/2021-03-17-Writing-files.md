---
title: '파일 쓰기'
excerpt: '자바 프로그램에서 텍스트를 파일에 쓰는 방법을 알아보자'

categories:
  - Java
tags:
  - Working with data
  - File processing

toc: true
toc_label: 'Table of contents'
toc_sticky: true
---

> 출처 [Writing files - JetBrains Academy](https://hyperskill.org/learn/step/3652)를 보고 요약한 내용입니다.

자바는 파일에 텍스트를 쓰는 다양한 방법을 제공합니다. 그 중 가장 간단한 방법으로 `java.io.FileWriter`와 `java.io.PrintWriter`가 있습니다.

## 1. The FileWriter class

`FileWriter`는 특정 파일에 문자들과 문자열을 쓰기위한 생성자들의 집합을 가지고 있습니다.

- `FileWriter(String filename);`
- `FileWriter(String fileName, boolean append);`
- `FileWriter(File file);`
- `FileWriter(File, file, boolean append);`

`append`는 기존 파일에 추가(`true`)해서 작성할지 덮어쓸지(`false`)를 나타내는 파라미터입니다.

**생성자가 `IOException`을 던지는 원인**

- 파일이 존재하지만 디렉토리일 때
- 파일이 존재하지 않고 생성할 수 없을 때
- 파일이 존재하지만 열 수 없을 때

**파일 생성 후 작성하기**

```java
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;

public class WritingFiles {

    public static void main(String[] args) throws IOException {
        File file = new File("/Users/ghYoon93/Documents/file.txt");
        FileWriter fileWriter = new FileWriter(file);

        fileWriter.write("Hello");
        fileWriter.write("Java");

        fileWriter.close();
    }
}
```

생성자의 인자로 넘긴 경로의 파일(`file.txt`)이 존재하지 않을 때 코드를 실행하면 파일을 생성합니다. 파일이 존재하면 이 코드는 파일의 데이터를 덮어씁니다.

**결과**

![FileWriter](/assets/images/FileWriter.png)

만약 새로운 데이터를 추가하고 싶다면 `FileWriter`의 인스턴스 생성 시 두 번째 인자를 `true`로 지정해야합니다.

```java
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;

public class WritingFiles {

    public static void main(String[] args) throws IOException {
        File file = new File("/Users/ghYoon93/Documents/file.txt");
        FileWriter fileWriter = new FileWriter(file, true);

        fileWriter.write("Hello World\r\n");
        fileWriter.close();
    }
}
```

위 코드는 파일에 새로운 라인을 추가합니다. 코드를 여러번 실행하면 다음과 같은 결과가 나옵니다.

![FileWriter-append](/assets/images/FileWriter-append.png)

**운영체제에 따른 줄바꿈 문자**

- `\n`: Unix-like
- `\r\n`: Windows

## 2. Closing a FileWriter

자원의 누수를 방지하기 위해 `FileWriter`을 쓴 다음 닫아줘야합니다.

```java
fileWriter.close();
```

자바 7부터는 **try-with-resources** 구문으로 `FileWriter`의 객체를 닫을 수 있습니다.

```java
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;

public class WritingFiles {

    public static void main(String[] args) throws IOException {
        File file = new File("/Users/ghYoon93/Documents/file.txt");

        try(FileWriter fileWriter = new FileWriter(file)) {
    		    fileWriter.write("Hello, World");
		    } catch (IOException e) {
		        System.out.printf("An exception occurs %s", e.getMessage());
		    }
    }
}
```

**try-with-resources** 구문이 끝나면 `FileWriter`의 객체가 자동으로 닫힙니다.

## 3. The PrintWriter Class

`PrintWriter` 클래스를 사용하면 파일에 형식적인 데이터를 쓸 수 있습니다.

**`PrintWriter`가 출력하는 데이터 타입**

- 문자열
- 원시 타입
- 문자 배열

**`PrintWriter`가 제공하는 오버라이드 메서드**

- `print`
- `println`
- `printf`

```java
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;

public class WritingFiles {

    public static void main(String[] args) throws IOException {
        File file = new File("/Users/ghYoon93/Documents/file.txt");

        try(PrintWriter printWriter = new PrintWriter(file)) {
            printWriter.print("Hello"); // 문자열 출력
            printWriter.println("Java"); // 문자열 출력 후 줄 바꿈
            printWriter.println(123); // 숫자 출력
            printWriter.printf("You have %d %s", 400, "gold coins"); // 형식적인 문자열 출력
        } catch (IOException e) {
            System.out.printf("An exception occurs %s", e.getMessage());
        }
    }
}
```

**결과**

![PrintWriter](/assets/images/PrintWriter.png)

**`PrintWriter` 생성자**

- `PrintWriter(String fileName);`
- `PrintWriter(File file);`

또한 `FileWriter`의 부모 클래스인 `Writer` 추상 클래스를 이용해서 `FileWriter`를 인자로 쓸 수 있습니다.

- `PrintWriter(Writer writer);`

`FileWriter`와 `PrintWriter`는 `Writer`를 확장하고 있어 유사점이 많습니다. 그러나 `PrintWriter`은 좀 더 고급 수준에 가깝고 유용한 메서드를 제공합니다. 그 중 원시 타입의 데이터를 쓰기 위한 포매팅 메서드와 오버로드된 프린트 메서드들이 있습니다.
