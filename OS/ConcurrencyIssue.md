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
> &nbsp;&nbsp; synchronized를 사용한 방법
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1. synchronized 키워드 사용 (거의 사용 X)
> 
> &nbsp;&nbsp; DB(MySQL)를 사용한 방법
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2. Pessimistic Lock
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 3. Optimistic Lock
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 4. Named Lock
> 
> &nbsp;&nbsp; Redis를 사용한 방법
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 5. Lettuce 사용하기
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 6. Redisson 사용하기

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
<hr/>

### ② [MySQL] Pessimistic Lock 적용

![image](https://github.com/user-attachments/assets/4987781d-5125-46af-bae6-999b51d53753)

- 실제로 데이터에 Lock을 걸어 데이터 정합성을 맞춘다.
- Row / Table 단위로 건다.
- 장점
    - Exclusive Lock을 거는 경우, 다른 트랜잭션에서는 Lock이 해제되기 전 데이터를 가져갈 수 없다. (=데이터 정합성이 보장된다.)
    - 충돌이 빈번하게 일어난다면, Opitimistic Lock 보다 성능이 좋을 수 있다.
- 단점
    - 데드락이 걸릴 수 있어 주의해야 한다.
    - 별도의 Lock을 잡으므로 성능 감소가 있을 수 있다.

<br/>

#### How To
1. Pessimistic Lock을 설정한다. ⇒ Repository 인터페이스에 ```@Lock``` 애노테이션을 건다. 
```java
public interface StockRepository extends JpaRepository<Stock, Long> {

    @Lock(LockModeType.PESSIMISTIC_WRITE)  // ⚠️ Pessimistic Lock 걺
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

### ③ [MySQL] Optimistic Lock 적용

![image](https://github.com/user-attachments/assets/94f19de1-e402-4143-a6a4-b3409fc3bd41)


- 실제로 Lock을 이용하지 않고 **버전**을 이용함으로써 데이터 정합성을 맞춘다. <br/>![image](https://github.com/user-attachments/assets/17c43967-6911-4d41-925c-2933abdee93a)

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

    @Lock(LockModeType.OPTIMISTIC)    // ⚠️ Optimisitc Lock 걺
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

### ④ [MySQL] Named Lock 적용
- 이름을 가진 메타데이터 Lock
- 주로 분산 Lock 구현 시 사용된다.
- 데이터 삽입 시 데이터 정합성 맞추기 위해 사용된다.
- 이름을 가진 Lock을 획득한 후, 해제할 때까지 다른 세션은 이 Lock을 획득할 수 없다.
- [주의사항] 트랜잭션 종료 시 Lock이 자동으로 해제되지 않으므로, 별도의 명령어로 해제를 수행하거나, 선점 시간이 끝나야 Lock이 해제된다.

<br/>

<b>예</b><br/>

![image](https://github.com/user-attachments/assets/122d83ee-7d98-416d-a0b4-a8a667faefff)

- 대상이 아닌 별도의 공간에 Lock을 건다.
- 세션1이 1이라는 이름으로 Lock을 건다면, 다른 세션은 세션1이 해제된 이후 Lock을 걸 수 있다.

<br/>

<b>장점</b><br/>
- 비관적 락은 타임아웃 구현이 어렵지만, 네임드 락은 타임아웃 구현이 쉽다.
    
<b>단점</b><br/>
- 트랜잭션 종료 시 Lock 해제, 세션 관리를 잘 해줘야 하므로 주의해서 사용해야 한다.
- 실제로 사용할 때는 구현 방법이 복잡할 수 있다.

<br/>

#### How To
- MySQL > ```get_lock``` : 네임드 락 획득, ```release_lock``` : 네임드 락 해제
- 편의성을 위해 JPA의 Native Query를 사용한다.
- 이번에는 동일한 데이터소스를 이용해 구현한다. (다른 서비스에도 영향을 미칠 수 있으므로 평소에는 데이터소스를 분리해 구현하는 것을 권장)

<br/>

1. Lock 관련 레포지토리를 생성해 Named Lock을 설정한다. (```repository/LockRepository.interface```)
```java
package com.example.stock.repository;

import com.example.stock.domain.Stock;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;

public interface LockRepository extends JpaRepository<Stock, Long> { // 실무에서는 Stock이 아닌 별도의 JDBC 사용해야 함

    // 네임드 락 획득
    @Query(value = "select get_lock(:key, 3000)", nativeQuery = true)
    void getLock(String key);

    // 네임드 락 해제
    @Query(value = "select release_lock(:key)", nativeQuery = true)
    void releaseLock(String key);
}
```

<br/>

2. 실제 로직 전후로 Lock을 획득-해제 해야 하므로 facade 클래스를 추가한다. (```facade/NamedLockStockFacade.java```)
```java
package com.example.stock.facade;

import com.example.stock.domain.Stock;
import com.example.stock.repository.LockRepository;
import com.example.stock.service.StockService;
import org.springframework.stereotype.Service;

@Service
public class NamedLockStockFacade {

    private LockRepository lockRepository;  // Lock 획득용

    private final StockService stockService; // 재고 감소용

    public NamedLockStockFacade(LockRepository lockRepository, StockService stockService) {
        this.lockRepository = lockRepository;
        this.stockService = stockService;
    }

    public void decrease(Long id, Long quantity) {
        try {
            lockRepository.getLock(id.toString());  // Lock 획득하기
            stockService.decrease(id, quantity);    // 재고 감소하기
        } finally {
            // 모든 로직 종료 시 Lock 해제시키기
            lockRepository.releaseLock(id.toString());
        }
    }
}
```

<br/>

3. StockService에서는 부모 트랜잭션과 별도로 실행되어야 하므로 ```propagation```을 변경하여 Named Lock을 위한 재고 감소 로직을 작성한다. (```service/StockService.java```)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - 원래는 따로 만들어야 한다.
```java
package com.example.stock.service;

import com.example.stock.domain.Stock;
import com.example.stock.repository.StockRepository;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;

@Service
public class StockService {

    private final StockRepository sr;

    public StockService(StockRepository sr) {
        this.sr = sr;
    }

    // 재고 감소 메소드
    @Transactional(propagation = Propagation.REQUIRES_NEW)    // ⚠️ 부모 트랜잭션과 별도로 실행되어야 하므로 propagation 변경
    public void decrease(Long id, Long quantity) {
        Stock stock = sr.findById(id).orElseThrow();    // Stock 조회하고
        stock.decrease(quantity);   // 재고 감소시킨 뒤
        sr.saveAndFlush(stock);    // 갱신된 값 저장하기
    }
}
```

<br/>

4. 같은 데이터소스를 사용할 것이므로 커넥션 풀 사이즈를 넉넉하게 변경한다. (```application.yml```)
```yaml
spring.datasource.hikari.maximum-pool-size: 40
```

<br/>

5. 낙관적 락 테스트코드와 거의 유사하게 [테스트코드](https://github.com/bono039/stock/blob/main/src/test/java/com/example/stock/facade/NamedLockStockFacadeTest.java)를 작성하고 실행한다. (```test/.../facade/NamedLockStockFacadeTest.java```)

<br/>


#### 결과
![image](https://github.com/user-attachments/assets/10804e43-f827-4aae-9412-c1ee1b7d75eb)

<br/>

<hr/>

### ⑤ [Redis] Lettuce 활용
- MySQL의 Named Lock과 유사
  - [차이점] Redis를 사용하며, 세션 관리에 신경쓰지 않아도 된다.
- 재시도가 필요하지 않은 경우 활용
- 장점
  - 구현이 간단하다.
  - spring data redis 이용 시, Lettuce가 기본이므로 별도 라이브러리를 사용하지 않아도 된다.
- 단점
  - 스핀락 방식이므로 동시에 많은 스레드가 Lock 획득-대기 상태라면, Redis에 부하가 될 수 있다.
      - [극복] ```Thread.sleep()```을 통해 Lock의 획득-재시도 간 term을 둬야 함  

<br/>

#### How to
- Redis 환경 설정 방법은 [해당 글](https://github.com/bono039/TIL/blob/main/Redis/Redis.md) 참고

<br/>

1. cmd에서 ```docker ps``` 명령어 실행 후, Redis 컨테이너 id 복사하기

![image](https://github.com/user-attachments/assets/e4b67f9e-333b-4e0f-890c-5bf39c51fad1)


<br/>

2. redis-cli 실행하기
```bash
$ docker exec -it 컨테이너ID redis-cli
```

![image](https://github.com/user-attachments/assets/1e5b09e4-5f4e-4666-b6e8-3aa9c0b36993)

<br/>

3. key가 1인 데이터를 ```setnx```하기
- _처음에는 key가 1인 데이터가 없으므로 성공_
- 기존에 있는 데이터라면 ```$ redis-cli FLUSHALL``` 명령어를 실행해 Redis 데이터 초기화 후 실행하기
```bash
$ setnx 1 lock
```

![image](https://github.com/user-attachments/assets/0fbc859f-661b-43dc-9831-f7035b4441f3)

<br/>

4. Redis 명령어를 사용하도록 Redis Repository를 생성한다. (```repository/RedisLockRepository.java```)
```java
package com.example.stock.repository;

import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;

import java.time.Duration;

@Component
public class RedisLockRepository {

    // ⚠️ Redis 명령어 사용하고자 레디스 템플릿 추가
    private RedisTemplate<String, String> redisTemplate;

    public RedisLockRepository(RedisTemplate<String, String> redisTemplate) {
        this.redisTemplate = redisTemplate;
    }

    // Lock 메소드 (로직 실행 전 실행되며 lock 걺)
    public Boolean lock(Long key) {
        return redisTemplate
                .opsForValue()
                .setIfAbsent(generateKey(key), "lock", Duration.ofMillis(3_000));
    }

    // Unlock 메소드 (로직 끝나면 실행되며 lock 해제)
    public Boolean unlock(Long key) {
        return redisTemplate.delete(generateKey(key));
    }

    private String generateKey(Long key) {
        return key.toString();
    }
}
```

<br/>

5. 로직 실행 전후로 Lock 획득-해제를 해줘야 하므로 facade 클래스를 생성한다. (```facade/LettuceLockStockFacade.java```)
```java
package com.example.stock.facade;

import com.example.stock.repository.RedisLockRepository;
import com.example.stock.service.StockService;
import org.springframework.stereotype.Service;

@Service
public class LettuceLockStockFacade {

    private final RedisLockRepository redisLockRepository;    // Redis 사용해 Lock 설정용
    private final StockService stockService;  // 재고 감소용

    // 생성자
    public LettuceLockStockFacade(RedisLockRepository redisLockRepository, StockService stockService) {
        this.redisLockRepository = redisLockRepository;
        this.stockService = stockService;
    }

    public void decrease(Long id, Long quantity) throws InterruptedException {
        while (!redisLockRepository.lock(id)) { // Lock 획득 시도
            Thread.sleep(100);
        }

        try {
            // Lock 획득 성공 시
            stockService.decrease(id, quantity);
        } finally {
            // 로직 모두 종료 시, unlock 메소드 활용해 Lock 해제
            redisLockRepository.unlock(id);
        }
    }
}
```

<br/>

6. 기존 테스트코드와 거의 유사하게 [테스트코드](https://github.com/bono039/stock/blob/main/src/test/java/com/example/stock/facade/LettuceLockStockFacadeTest.java)를 작성하고 실행한다. (```test/.../facade/LettuceLockStockFacadeTest.java```)

<br/>

### ⑥ [Redis] Redisson 활용
- Lokc 획득 재시도를 기본으로 제공한다. (∴ _실무에서 재시도가 필요한 경우 활용_)
   - 자신이 점유하고 있는 Lock 해제 시, 채널에 메시지를 보내줌으로써 Lock을 획득해야 하는 스레드들에게 Lock을 획득하라고 전달해준다
   - 그럼 메시지 받은 스레드들은 lock 획득을 재시도하게 된다.
- 장점
   - pub-sub 이용해 Redis 부하를 줄일 수 있다.
- 단점
   - 구현이 조금 복잡하다.
   - 별도의 라이브러리를 사용해야 한다.
   - Lock을 라이브러리 차원에서 제공하므로 사용법을 배워야 한다.
- vs ```Lettuce```
   - ```Lettuce```는 계속 Lock을 시도하지만, ```Redisson```은 Lock 해제 시 한 번 or 몇 번만 시도하면서 Redis 부하를 줄인다.

<br/>

#### How to
- Redisson은 Lock 관련 클래스들을 라이브러리에서 제공하므로 별도 레포지토리는 생성하지 않는다.
- 하지만, 로직 실행 전후로 Lock 획득-해제가 필요하므로 ```facade 클래스```를 이용한다.


<br/>

1. [mvnrepository 사이트](https://mvnrepository.com/artifact/org.redisson/redisson-spring-boot-starter/3.35.0)에서 버전에 맞는 Redisson 라이브러리를 탐색한다.
   
![image](https://github.com/user-attachments/assets/5926c575-1c09-4216-8d54-e98dddef0467)


<br/>

2.```build.gradle``` 파일에 위에서 찾은 의존성을 추가한다.
```gradle
implementation 'org.redisson:redisson-spring-boot-starter:3.17.4'
```

<br/>

3. 로직 실행 전후로 Lock 획득-해제가 필요하므로 facade 클래스를 이용한다. (```facade/RedissonLockStockFacade.java```)
```java
package com.example.stock.facade;

import com.example.stock.service.StockService;
import org.redisson.api.RLock;
import org.redisson.api.RedissonClient;
import org.springframework.stereotype.Component;

import java.util.concurrent.TimeUnit;

@Component
public class RedissonLockStockFacade {

    private RedissonClient redissonClient;  // Lock 획득용 (Redisson 라이브러리 덕에 Repository 따로 안 만들어도 됨)
    private StockService stockService;  // 재고 감소용

    public RedissonLockStockFacade(RedissonClient redissonClient, StockService stockService) {
        this.redissonClient = redissonClient;
        this.stockService = stockService;
    }

    // 재고 감소 로직
    public void decrease(Long id, Long quantity) {
        RLock lock = redissonClient.getLock(id.toString());   // Lock 객체 가져오기

        try {
            boolean available = lock.tryLock(10, 1, TimeUnit.SECONDS);

            // 몇 초 동안 lock 획득하고 점유할 것인지 설정하기
            if(!available) {
                System.out.println("lock 획득 실패");
                return;
            }

            // 정상적으로 Lock 획득한 경우
            stockService.decrease(id, quantity);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        } finally {
            // 로직 모두 정상적으로 종료되면, Lock 해제하기
            lock.unlock();
        }
    }
}
```

<br/>

4. 기존 테스트코드와 거의 유사하게 [테스트코드](https://github.com/bono039/stock/blob/main/src/test/java/com/example/stock/facade/RedissonLockStockFacadeTest.java)를 작성하고 실행한다. (```test/.../facade/RedissonLockStockFacadeTest.java```)
- [주의] TC 실행 시 ```org.opentest4j.AssertionFailedError```가 발생하는 경우, ```$ redis-cli FLUSHALL``` 명령어를 실행해 Redis 데이터 초기화 후 실행하기

<br/>


## 🔗 참고
* https://www.inflearn.com/course/%EB%8F%99%EC%8B%9C%EC%84%B1%EC%9D%B4%EC%8A%88-%EC%9E%AC%EA%B3%A0%EC%8B%9C%EC%8A%A4%ED%85%9C/news
* https://helloworld.kurly.com/blog/distributed-redisson-lock/
