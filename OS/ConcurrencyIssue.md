# 동시성 이슈 해결법
> 
> [발생 원인] Race Condition
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

## [발생 원인] Race Condition
### 정의
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

<br/>

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

### ② Pessimistic Lock 적용

![image](https://github.com/user-attachments/assets/4987781d-5125-46af-bae6-999b51d53753)

- 실제로 데이터에 Lock을 걸어 데이터 정합성을 맞춘다.
- Row / Table 단위로 건다.
- 장점
    - Exclusive Lock을 거는 경우, 다른 트랜잭션에서는 Lock이 해제되기 전 데이터를 가져갈 수 없다. (=데이터 정합성이 보장된다.)
    - 충돌이 빈번하게 일어난다면, Opitimistic Lock 보다 성능이 좋을 수 있다.
- 단점
    - 데드락이 걸릴 수 있어 주의해야 한다.
    - 별도의 Lock을 잡으므로 성능 감소가 있을 수 있다.

![image](https://github.com/user-attachments/assets/188c4923-3a1b-4364-82d2-da506eba9840)
![image](https://github.com/user-attachments/assets/17c43967-6911-4d41-925c-2933abdee93a)

<br/>

#### How To
1. Pessimistic Lock을 설정한다. ⇒ Repository 인터페이스에 ```@Lock``` 애노테이션을 건다. 
```java
public interface StockRepository extends JpaRepository<Stock, Long> {

    @Lock(LockModeType.PESSIMISTIC_WRITE)  // Pessimistic Lock 걺
    @Query("select s from Stock s where s.id = :id")
    Stock findByIdWithPessimisticLock(Long id);
}
```

<br/>

2. Pessimistic Lock을 위한 재고 감소 로직을 작성한다. (```service/PessimisticLockStockService.java```)
```java
package com.example.stock.service;

import com.example.stock.domain.Stock;
import com.example.stock.repository.StockRepository;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class PessimisticLockStockService {

    private final StockRepository stockRepository;

    public PessimisticLockStockService(StockRepository stockRepository) {
        this.stockRepository = stockRepository;
    }

    @Transactional
    public void decrease(Long id, Long quantity) {
        Stock stock = stockRepository.findByIdWithPessimisticLock(id);  // Lock 활용해서 데이터 가져오기

        stock.decrease(quantity);   // 재고 감소시키기

        stockRepository.save(stock);    // 데이터 저장하기
    }
}
```

<br/>

3. TestCase에서 StockServiceTest 코드를 아래와 같이 수정한다.
> (Before) private **StockService** stockService;
> 
> (After) private **PessimisticLockStockService** stockService;

<br/>

#### 결과
![image](https://github.com/user-attachments/assets/dfdd5e68-76a1-4893-b62e-36a8b3bea855)

<br/>


### ③ Optimistic Lock 적용

![image](https://github.com/user-attachments/assets/94f19de1-e402-4143-a6a4-b3409fc3bd41)


- 실제로 Lock을 이용하지 않고 **버전**을 이용함으로써 데이터 정합성을 맞춘다.

- 장점
    - 별도의 Lock을 잡지 않아 비관적 락보다는 성능 상 좋다.
    - 충돌이 빈번하게 일어나지 않는 경우 추천
- 단점
    - 업데이트가 실패 시, 재시도 로직을 개발자가 직접 작성해야 한다.
    - 충돌이 빈번하게 일어나는 경우에는 비추천 (이 때는 비관적 락 추천)

<br/>

#### How To
1. 버전을 활용할 것이므로 도메인 클래스에 아래 코드를 삽입한다. (```domain/Stock.java```)
```java
@Version
private Long version;
```
<br/>

2. Optimistic Lock을 설정한다. ⇒ Repository 인터페이스에 ```@Lock``` 애노테이션을 건다.
```java
public interface StockRepository extends JpaRepository<Stock, Long> {

    @Lock(LockModeType.OPTIMISTIC)    // Optimisitc Lock 걺
    @Query("select s from Stock s where s.id = :id")
    Stock findByIdWithOptimisticLock(Long id);
}
```

<br/>

3. Optimistic Lock을 위한 재고 감소 로직을 작성한다. (```service/OptimisticLockStockService.java```)
```java
package com.example.stock.service;

import com.example.stock.domain.Stock;
import com.example.stock.repository.StockRepository;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class OptimisticLockStockService {

    private StockRepository stockRepository;

    public OptimisticLockStockService(StockRepository stockRepository) {
        this.stockRepository = stockRepository;
    }

    @Transactional
    public void decrease(Long id, Long quantity) {
        Stock stock = stockRepository.findByIdWithOptimisticLock(id);   // Optimistic Lock으로 데이터 가져오기

        stock.decrease(quantity);    // 재고 감소시키기
        stockRepository.save(stock);    // 데이터 저장하기
    }
}
```

<br/>

4. 업데이트 실패 시 재시도해야 하므로 facade를 사용한다. (```facade/OptimisticLockStockFacade.java```)
```java
package com.example.stock.facade;

import com.example.stock.service.OptimisticLockStockService;
import org.springframework.stereotype.Service;

@Service
public class OptimisticLockStockFacade {

    private final OptimisticLockStockService optimisticLockStockService;

    public OptimisticLockStockFacade(OptimisticLockStockService optimisticLockStockService) {
        this.optimisticLockStockService = optimisticLockStockService;
    }

    public void decrease(Long id, Long quantity) throws InterruptedException {
        // 업데이트 실패 시 재시도해야 하므로 while문 사용
        while(true) {
            try {
                optimisticLockStockService.decrease(id, quantity);
                break;
            } catch(Exception e) {
                // 업데이트 실패 시, 50ms 이후 재시도
                Thread.sleep(50);
            }
        }
    }
}
```

<br/>

5. 비관적 락 테스트코드와 거의 유사하게 [테스트코드](https://github.com/bono039/stock/blob/main/src/test/java/com/example/stock/facade/OptimisticLockStockFacadeTest.java)를 작성하고 실행한다. (```test/.../facade/OptimisticLockStockFacadeTest.java```)

<br/>


#### 결과
![image](https://github.com/user-attachments/assets/b192ee3a-5759-417d-8dca-4db6f0428cc1)





<br/>

## 🔗 참고
