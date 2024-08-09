## 에러
스프링 부트 프로젝트를 빌드하려고 했는데 아래와 같은 에러가 났다.
```
> Could not resolve all files for configuration ':testCompileClasspath'.
  > Could not find snakeyaml-2.2-android.jar (org.yaml:snakeyaml:2.2).
    Searched in the following locations:
```

![image](https://github.com/user-attachments/assets/ce96c161-73fa-4e5c-add8-740228040a1b)


<br/>

## 해결 방법
<code>build.gradle</code> 파일에 아래 코드를 추가하고 import한다.

```
// Faker : 테스트 시 가짜 데이터 생성하는 오픈소스 라이브러리
implementation ('com.github.javafaker:javafaker:1.0.2') {exclude module: 'org.yaml'}
implementation group: 'org.yaml', name: 'snakeyaml', version: '1.26'
```

<br/>

## 결과
해당 에러는 더 이상 나타나지 않게 된다.

<br/>

## 🔗 참고
* https://github.com/DiUS/java-faker/issues/327

<br/>
