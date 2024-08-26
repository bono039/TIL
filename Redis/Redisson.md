# Redisson에서 사용하는 pub-sub 사용해보기

### 1. cmd에서 redis-cli 실행하기
```bash
$ docker exec -it 컨테이너명 redis-cli
```

<br/>

### 2. redis-cli가 2개 필요하므로 터미널을 2개 켜서 redis-cli 실행하기
- A : 채널 ch1 구독하기 ⇒ ```subscribe ch1```
- B : 채널 ch1에 hello 메시지 보내기 ⇒ ```publish ch1 hello```

![image](https://github.com/user-attachments/assets/f318db45-5cad-4e91-99ef-42e2280f920c)
