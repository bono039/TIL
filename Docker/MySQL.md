
# MySQL 작업 환경 세팅
> MySQL 설치 및 실행
> 
> MySQL 데이터베이스 생성
<br/>

## MySQL 설치 및 실행
* MySQL 설치
```bash
docker pull mysql
```

<img src="https://github.com/user-attachments/assets/bc0ea9e2-6b6b-463f-a3bc-5c4bf325bf94" alt="clock" width="500"/>

<br/>
<br/>
<br/>

* MySQL 데이터베이스 컨테이너 mysql1 생성 및 실행 (로컬 머신의 **127.0.0.1:3307**에서 MySQL에 접근)
```bash
docker run -d -p 3307:3306 -e MYSQL_ROOT_PASSWORD=1234 --name mysql1 mysql
```
- ```docker run``` : Docker 컨테이너 실행 위한 명령어
- ```-d``` : 컨테이너를 백그라운드에서 실행하도록 지정
- ```-p 호스트포트:컨테이너포트``` : 호스트(=로컬 머신)의 포트 3307을 컨테이너 내부에서 MySQL이 사용하는 기본 포트 3306에 바인딩
  - 잘못 설정 시, ```com.mysql.cj.jdbc.exceptions.CommunicationsException: Communications link failure``` 에러가 발생한다.
- ```-e MYSQL_ROOT_PASSWORD=1234``` : 컨테이너에서 실행될 MySQL의 환경 변수 설정. MySQL의 루트(root) 계정 비밀번호를 1234로 설정.
- ```--name mysql1``` : 생성될 컨테이너에 mysql1이라는 이름 부여
- ```--mysql``` : 'mysql' 이미지로 컨테이너 생성


<br/>
<br/>
<br/>

* 컨테이너 mysql1이 정상적으로 실행되는지 확인

```bash
docker ps
```
![image](https://github.com/user-attachments/assets/5c44d020-ec29-4d2f-b6a2-12457e6fe2bb)

<br/>
<br/>
<br/>

## MySQL 데이터베이스 생성
* MySQL 실행 - bash로 진입 (exec)
```bash
docker exec -it mysql1 bash
```

<br/>

* root 계정으로 접속해 비밀번호 입력
```bash
mysql -u root -p
```

<img src="https://github.com/user-attachments/assets/e2da0bdd-293a-4066-b9bc-2870f8d50d18" alt="clock" width="550"/>

<br/>
<br/>
<br/>

* 데이터베이스 stock_example 생성
```MySQL
create database stock_example;
```
<img src="https://github.com/user-attachments/assets/7ca96998-2a0b-4bcf-810a-cc46a1930d0f" alt="clock" width="350"/>

<br/>
<br/>
<br/>

* 정상적으로 생성되었는지 확인
```MySQL
use stock_example;
```
<img src="https://github.com/user-attachments/assets/1db1946e-2523-4d3c-92ba-cda5c74a28e6" alt="clock" width="300"/>

<br/>
<br/>
<br/>

참고
* https://2nan.tistory.com/107
* https://www.inflearn.com/community/questions/1233570/%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EB%A5%BC-%EC%B2%98%EC%9D%8C-%EC%8B%9C%EC%9E%91%EC%8B%9C%EC%97%90-java-sql-sqlexception-access-denied-for-user-x27-root-x27-x
