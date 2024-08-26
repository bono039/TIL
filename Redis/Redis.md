
# Redis
> Redis 라이브러리란?
> 
> 종류
> 
> 작업환경 세팅
<br/>

## Redis 라이브러리란?
> 분산 Lock 구현을 위해 사용하는 대표적 라이브러리

<br/>

## 종류
### 1. _**Lettuce**_<br/><img src="https://github.com/user-attachments/assets/ccc270c2-ed0d-472e-b97b-9c454e14fd46" width="500" height="300" />
- ```setnx``` 명령어를 통해 분산Lock 구현
- 스핀락 방식

<br/>

### 2. _**Redisson**_<br/>![image](https://github.com/user-attachments/assets/848cc373-3cb1-4400-8542-b13659050a08)
- pub-sub 기반으로 Lock 구현 제공

<br/>

## 작업환경 세팅

### 1. cmd 켜서 Redis 라이브러리 다운받기
```bash
$ docker pull redis
```
![image](https://github.com/user-attachments/assets/3d1606b9-3334-4fd9-80ae-5fc3378623de)

<br/>

### 2. Redis 실행하기
```bash
$ docker run --name myredis -d -p 6379:6379 redis
```
![image](https://github.com/user-attachments/assets/06744608-1828-4dd4-866e-ba72532fea17)

<br/>

### 3. Redis가 정상실행되는지 확인하기
```bash
$ docker ps
```
![image](https://github.com/user-attachments/assets/cd91d343-b79f-434c-8a5a-50b8e37979ca)

<br/>


## 참고
* https://www.inflearn.com/course/%EB%8F%99%EC%8B%9C%EC%84%B1%EC%9D%B4%EC%8A%88-%EC%9E%AC%EA%B3%A0%EC%8B%9C%EC%8A%A4%ED%85%9C/news
