## 에러
JavaFaker를 사용하여 더미 데이터를 만들고 테스트하려고 했는데 다음과 같은 에러가 났다.
```
Caused by: java.lang.ClassNotFoundException: org.yaml.snakeyaml.inspector.TagInspector
	at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:641)
	at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:188)
	at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:525)
	... 107 common frames omitted
```

<br/>

## 원인
Spring Boot 버전과 snakeYAML 버전이 맞지 않아 발생하는 에러다.

<br/>

## 해결 방법
1. [해당 사이트](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter)에 접속한다.
2. 현재 Spring Boot 버전을 누른다. 

![image](https://github.com/user-attachments/assets/d69ceb5c-77e6-4c9b-bb29-fb2e49e6d734)

<br/>

3. 접속한 화면을 아래로 내려 ```Compile Dependencies```에서 맞는 snakeyaml 버전을 확인한다.
![image](https://github.com/user-attachments/assets/6e26fa7d-1078-4916-bb09-00a1d3f030a0) ![image](https://github.com/user-attachments/assets/2a54ecec-ccf5-4354-afcc-38e3be7f4d8d)


<br/>

4. <code>build.gradle</code> 파일에서 현재 Spring Boot 버전 3.3.2와 맞는 snakeYAML 버전으로 변경한다.

```
plugins {
	id 'java'
	id 'org.springframework.boot' version '3.3.2'
	id 'io.spring.dependency-management' version '1.1.6'
}

...

// Faker : 테스트 시 가짜 데이터 생성하는 오픈소스 라이브러리
implementation ('com.github.javafaker:javafaker:1.0.2') {exclude module: 'snakeyaml'}
implementation (group: 'org.yaml', name: 'snakeyaml', version: '2.2')
```

<br/>

## 결과
에러를 없애고 테스트코드를 정상 실행할 수 있다.

![image](https://github.com/user-attachments/assets/1a95eb4a-adf9-4715-8b5b-22f583b8999a)


<br/>

## 🔗 참고
* https://velog.io/@wongi-kim/JavaFaker-%ED%8A%B8%EB%9F%AC%EB%B8%94-%EC%8A%88%ED%8C%85

<br/>
