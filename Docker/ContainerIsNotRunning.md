## 에러
docker 컨테이너에 접속하려고 ```docker exec -it mysql1 bash``` 명령어를 보냈는데 아래와 같이 에러가 떴다.

```
Error response from daemon: container ~ is not running
```

![image](https://github.com/user-attachments/assets/bfa3ebd5-c0c3-4858-a4a2-f05c70c40dc7)

<br/>

## 원인
컨테이너가 실행중이 아니라 발생하는 에러였다.

<br/>

## 해결 방법
컨테이너를 실행하고 다시 명령어를 입력하면 된다.
```bash
docker container start 컨테이너명
```
![image](https://github.com/user-attachments/assets/9f419c4e-b9f6-4f1a-9f34-9917eb98800c)


<br/>

<br/>



<b>🔴 유의사항</b>

- 이것 때문에 com.mysql.cj.jdbc.exceptions.CommunicationsException: Communications link failure 에러가 발생할 수 있다.

<br/>

## 🔗 참고
* https://nirsa.tistory.com/55

<br/>
