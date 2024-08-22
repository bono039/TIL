# Race Condition으로 인한 동시성 이슈 해결법
> 
> Race Condition
> 
> &nbsp;&nbsp; 정의
>
> &nbsp;&nbsp; 예시
>
> 
> 동시성 이슈 해결법
> 
> &nbsp;&nbsp; 1. synchronized 키워드 사용 (거의 사용 X)
> 
> &nbsp;&nbsp; 2. Pessimistic Lock
> 
> &nbsp;&nbsp; 3. Optimistic Lock
>
> &nbsp;&nbsp; 4. Named Lock

<br/>

## 🏃‍♀️ Race Condition
> 둘 이상의 스레드가 공유 데이터에 액세스할 수 있고, 동시에 변경하려고 할 때 발생하는 문제

<br/>

### 예시
```java
@Test
public void 동시에_100개의_요청() throws InterruptedException {
    int threadCnt = 100;
    ExecutorService executorService = Executors.newFixedThreadPool(32); // 비동기 실행 작업을 단순화해 사용할 수 있게 하는 API
    CountDownLatch latch = new CountDownLatch(threadCnt);   // 다른 스레드에서 수행 중인 작업이 완료될 때까지 대기하는 걸 돕는 클래스

    for(int i = 0 ;i < threadCnt ; i++) {
        executorService.submit(() -> {
            try {
                stockService.decrease(1L, 1L);
            }
            finally {
                latch.countDown();
            }
        });
    }

    latch.await();

    Stock stock = stockRepository.findById(1L).orElseThrow();
    // 100 - (1*100) = 0
    assertEquals(0, stock.getQuantity());
}
```

<b>예상</b><br/>
: 스레드1이 데이터를 가져가서 갱신한 값을 스레드2가 가져간 이후에 갱신하기를 예상한다.

![image](https://github.com/user-attachments/assets/7a11fe08-927a-462c-84d3-f82712ef95c4)


<b>현실</b><br/>
: 스레드 1이 데이터를 가져가서 갱신하기 전에 스레드2가 갱신되기 전 값을 가져가면서 갱신이 누락된다.

![image](https://github.com/user-attachments/assets/12fe319b-21c1-4d34-a6bd-e68249d2fcc9)

![image](https://github.com/user-attachments/assets/0fb11d0d-b2e3-40b6-8403-f8a360555176)


<br/>

## 동시성 이슈 해결법
하나의 스레드가 작업이 완료된 이후, 다른 스레드가 데이터에 접근할 수 있도록 한다.
<br/>
<br/>

### ① synchronized 키워드 사용 (거의 사용 X)
```Service``` 파일의 메소드 선언부에 ```synchronized``` 키워드를 붙이고, ```@Transactional``` 부분을 주석 처리해 한 개의 스레드만 접근 가능하게 한다.
![image](https://github.com/user-attachments/assets/b7bf9f6e-e1d2-482f-9f37-956b3a0bf5a5)


<details>
<summary>@Transactional 부분에 주석 처리하는 이유</summary>
<div markdown="1">

```@Transactional``` 동작 방식 때문에 에러가 그대로 발생하기 때문이다.</b>

- ```@Transactional``` 동작 방식이 어떻길래?
  - ```@Transactional```은 매핑한 클래스를 새로 만들어 실행한다.
  - 그리고 트랜잭션 종료 시점에 DB에 업데이트를 하게 되는데, 실제 DB가 업데이트되기 전에 다른 스레드에서도 메소드 호출이 가능하다.
  - 이 때, 다른 스레드에서 갱신되기 전 값을 가져가게 되면서 이전과 동일한 문제가 발생하게 되는 것이다.

</div>
</details>

<br/>

<b>결과</b> <br/>
: 테스트케이스가 정상적으로 실행된다.

![image](https://github.com/user-attachments/assets/2ad4da35-150c-44ea-b20d-1af08d750772)

<br/>

<b>문제점</b><br/>
: JAVA의 ```synchronized``` 키워드는 하나의 프로세스 안에서만 보장되는데,
서버가 2대 이상이면 여러 스레드에서 동시에 데이터에 접근 가능하게 된다. (Race Condition 이슈)

![image](https://github.com/user-attachments/assets/f5510bb5-c47c-4a39-a3b5-8ba120e2b7d1)



<br/>

### 🔗 참고
