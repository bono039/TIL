## 에러
스프링 부트 테스트코드를 실행하려고 했는데 대략 아래와 같은 에러가 났다.
```
com.mysql.cj.jdbc.exceptions.CommunicationsException: Communications link failure
```
<br/>

## 원인
Spring과 MySQL이 제대로 연결되지 않아 발생하는 에러다.
나의 경우, 포트번호를 3307로 설정하고자 했고, 이 때 docker run 명령에서 -p 부분을 제대로 작성하지 않아 발생하는 에러였다.

<br/>

## 해결 방법
1. <code>build.gradle</code> 파일에서 에러가 나는 부분이 있는지 확인한다.
```gradle
plugins {
	id 'org.springframework.boot' version '2.7.0'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '11'

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-data-redis'
	implementation 'org.springframework.boot:spring-boot-starter-jdbc'
	implementation 'org.springframework.boot:spring-boot-starter-web'

	runtimeOnly 'mysql:mysql-connector-java'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
	useJUnitPlatform()
}

```

<br/>

2. <code>application.yml</code> 파일에서 에러가 나는 부분이 있는지 확인한다.
   - 유의해야 할 부분 : spring.datasource 부분
   - 나의 경우, 포트번호를 3306이 아닌 3307로 변경했다.

```yml
spring:
  jpa:
    hibernate:
      ddl-auto: create
    show-sql: true
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3307/stock_example
    username: root
    password: 1234

logging:
  level:
    org:
      hibernate:
        SQL: DEBUG
        type:
          descriptor:
            sql:
              BasicBinder: TRACE
```

<br/>

3. 모든 이미지와 컨테이너를 삭제하고 다시 아래와 같이 명령어들을 작성한다.
```bash
docker pull mysql

docker run -d -p 3307:3306 -e MYSQL_ROOT_PASSWORD=1234 --name mysql1 mysql

docker exec -it mysql1 bash
```

```MySQL
mysql -u root -p

create database stock_example;

use stock_example;
```

![image](https://github.com/user-attachments/assets/d32b227e-3e79-41cb-a7ec-e17ea098bd1c)
![image](https://github.com/user-attachments/assets/4ada846a-c832-45a9-b561-d637516849a6)

<br/>

<b>🔴 유의사항</b>

- **docker run** 명령어 부분에서 **포트번호 부분**을 유의해야 한다.
 ![image](https://github.com/user-attachments/assets/f0d351d9-b30c-4245-9491-a36e39711f09)


<br/>


<br/>

## 결과
에러가 사라지고 테스트코드가 정상적으로 실행된다.
![image](https://github.com/user-attachments/assets/3b6bdd71-c46f-48b3-a05b-0a6bd046d300)

<br/>

## 🔗 참고
* https://github.com/bono039/TIL/blob/main/Docker/MySQL.md#mysql-%EC%9E%91%EC%97%85-%ED%99%98%EA%B2%BD-%EC%84%B8%ED%8C%85

<br/>
