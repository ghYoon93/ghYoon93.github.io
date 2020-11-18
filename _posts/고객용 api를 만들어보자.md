고객용 api를 만들어보자

프로젝트 분리 시 jdbc 문제와 포트 문제가 발생 (스프링 설정으로 해결할 수 있다)



Task: package:compileJava FAILED

package가 다른 패키지의 클래스를 필요로 하지만 제공 받지 못함

```yaml
bootJar {enable = true}
```

