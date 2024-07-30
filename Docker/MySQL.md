
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

* 포트 3307번에 컨테이너 mysql1 생성 및 실행
```bash
docker run -d -p 3307:3307 -e MYSQL_ROOT_PASSWORD=1234 --name mysql1 mysql
```

<img src="https://github.com/user-attachments/assets/92feea9b-7cbb-4a35-9831-bd02cebffe47" alt="clock" width="500"/>

<br/>
<br/>
<br/>

* 컨테이너 mysql1이 정상적으로 실행되는지 확인

```bash
docker ps
```
<img src="https://github.com/user-attachments/assets/cf6a3ba2-4a1f-4163-aa5b-317a9a06ca00" alt="clock" width="700"/>
<img src="https://github.com/user-attachments/assets/63b80cc3-df18-4484-9e4c-520a382c19e9" alt="clock" width="450"/>

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

