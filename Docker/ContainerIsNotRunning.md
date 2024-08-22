## 에러
테스트케이스가 실행이 되지 않으면서 ```com.mysql.cj.jdbc.exceptions.CommunicationsException: Communications link failure``` 에러가 발생했다.

docker 컨테이너에 접속하지 않아 발생하는 문제라는 사실을 깨닫고 ```docker exec -it mysql1 bash``` 명령어를 보냈는데, 아래와 같이 에러가 떴다.

```
Error response from daemon: container ~ is not running
```

![image](https://github.com/user-attachments/assets/bfa3ebd5-c0c3-4858-a4a2-f05c70c40dc7)

<br/>

## 원인
컨테이너가 실행중이 아니라 발생하는 에러였다.

<br/>

## 해결 방법
컨테이너를 실행하고 다시 나머지 명령어를 입력하면 된다.
```bash
docker container start 컨테이너명
```
![image](https://github.com/user-attachments/assets/f20d39c0-0940-403c-bc4b-e97076b609e3)



<br/>

## 결과
에러 없이 테스트케이스가 정상실행된다.

![image](https://github.com/user-attachments/assets/807fd737-1e66-41a7-a3b0-35349f20560b)



<br/>

## 🔗 참고
* https://nirsa.tistory.com/55

<br/>
