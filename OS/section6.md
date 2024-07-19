# 메모리와 캐시 메모리
> RAM의 특징과 종류
> 
> 메모리의 주소 공간
>
> 캐시 메모리

<br/>

## RAM의 특징과 종류
### 메모리
- 주기억장치 : **휘발성 저장 장치** ▷ **RAM** (→ 메모리), ROM
- 보조기억장치 : 비휘발성 저장 장치

### RAM 특징
- 휘발성 저장 장치 (전원 꺼지면 내용 날아감)
- 많은 프로그램들 동시 실행에 유리

### RAM 종류
#### 1. DRAM (Dynamic RAM)
- 저장된 데이터가 **동적으로 사라지는 RAM**
- 데이터 소멸 막기 위해 주기적으로 refresh해야 함
- 일반적으로 메모리로 사용되는 RAM
     - 상대적으로 소비전력이 낮고, 저렴하고, 집적도가 높아 대용량으로 설계하기 용이하므로

#### 2. SRAM (Static RAM)
- 저장된 데이터가 **정적인 (사라지지 않는) RAM**
- DRAM보다 더 빠름
- 상대적으로 소비전력이 높고, 비싸고, 집적도가 낮아 '대용량 설계 필요 없이 빨라야 하는 장치'에 사용
<img src="https://github.com/user-attachments/assets/95c6aa01-fa64-42ae-b7ce-f5a09ba87a20" alt="clock" width="450"/>

<br/>

#### 3. SDRAM (Synchronous RAM)
- 특별한(발전된 형태의) DRAM
- 클럭 신호와 동기화된 DRAM

<br/>
#### 4. DDR SDRAM (Double Data Rate SDRAM)
- 특별한(발전된 형태의) SDRAM
- 최근 가장 대중적으로 사용하는 RAM
- **대역폭**을 2배 넓혀 속도를 빠르게 만든 SDRAM &nbsp; (* _대역폭_ : 데이터를 주고받는 길의 너비)
          <details>
               <summary>각각 대역폭</summary>
               <div markdown="1">
               
     → SDR SDRAM : 대역폭 1개          
     → DDR SDRAM : 대역폭 2개          
     → DDR2 SDRAM : 대역폭 4개          
     → DDR3 SDRAM : 대역폭 8개          
     → DDR4 SDRAM : 대역폭 16개
               

<br/>

## 메모리의 주소 공간
### 물리 주소
- 메모리 입장에서 바라본 주소
- 정보가 실제로 저장된 HW 상 주소

### 논리 주소
- CPU와 실행 중인 프로그램 입장에서 바라본 주소
- 실행 중인 프로그램 각각에게 부여된 0번지부터 시작하는 주소
<img src="https://github.com/user-attachments/assets/45c67e98-d094-420a-bc45-5fdceeb8c8f1" alt="clock" width="450"/>

<br/>

<br/>

### 물리 주소와 논리 주소의 변환

    MMU(메모리 관리 장치)라는 HW 장치에 의해 변환

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBZu6r%2FbtsCTCuvCy3%2FzrdPzz19DAYGoGOYlgAP0k%2Fimg.png" alt="clock" width="350"/>

> MMU
> &nbsp; : 논리 주소와 베이스 레지스터의 값을 더해 논리 주소를 물리 주소로 변환

- ```논리 주소``` : 프로그램의 시작점으로부터 떨어진 거리
- ```베이스 레지스터``` : **프로그램의 가장 작은 물리 주소 = 프로그램의 첫 물리 주소**
- 단점 : 메모리 보호 NO

<img src="https://github.com/user-attachments/assets/92cdfff8-424f-4075-9ff3-ab042a4ae5d3" alt="clock" width="500"/>

<br/>

<br/>

### 메모리 보호 방법
#### 한계 레지스터
- 프로그램 영역 침범할 수 있는 명령어 실행 막음
- **논리 주소의 최대 크기 저장**
- 베이스 레지스터 값 ≤ 프로그램의 물리 주소 범위 < 베이스 레지스터 + 한계 레지스터 값
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbg13dU%2FbtsCZihGerw%2FsC5j51YNtrhTykNkNdFUFK%2Fimg.png" alt="clock" width="450"/>

- CPU, 메모리에 접근하기 전에 **접근하고자 하는 논리 주소가 한계 레지스터보다 작은지** 항상 검사
  <br/> ⇒ 실행 중인 프로그램의 독립적 실행 공간 확보 & 하나의 프로그램이 다른 프로그램 침범 못 하게 보호
<img src="https://github.com/user-attachments/assets/7c3a1d05-0a05-4155-b147-38d8727c8df5" alt="clock" width="500"/>

<br/>

<br/>

## 캐시 메모리 
### 저장 장치 계층 구조
1. CPU와 가까운 저장 장치는 빠르고, 멀리 있는 저장 장치는 느리다.
2. 속도가 빠른 저장 장치는 저장 용량이 작고, 가격이 비싸다.

<br/>

> 기준 : CPU에 얼마나 가까운지

<img src="https://github.com/user-attachments/assets/b8515661-c1f6-48d8-b0b9-9e0b1a166e7c" alt="clock" width="450"/>

<br/>

<br/>

### 캐시 메모리
    CPU의 연산 속도와 메모리 접근 속도의 차이를 줄이기 위한 저장 장치

- 레지스터(CPU)보다 용량이 크고 메모리보다 빠른 SRAM 기반 저장 장치 
- 메모리에서 CPU가 사용할 일부 데이터를 미리 캐시 메모리로 가져와 사용

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGtrli%2FbtsC1FQ0afu%2FRQtMMKle3fZxtIgyOgK1r1%2Fimg.png" alt="clock" width="450"/>

<br/>

#### 계층적 캐시 메모리 (L1-L2-L3 캐시)
> L1,L2 캐시는 코어 내부. L3 캐시는 코어 외부에 위치

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FEtff0%2FbtsCX6Paj3m%2FdIIggXFHGBbrQVGSGcO9k1%2Fimg.png" alt="clock" width="450"/>

- 속도 : L1 > L2 > L3 > 메모리
- 크기 : L1 < L2 < L3 < 메모리

<br/>

#### 분리형 캐시
- 더 빨리 담기 위해 데이터만 담고 있는 캐시 (L1D - Data), 명령어만 담고 있는 캐시 (L1I - Instruction)로 분리

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcxGX3I%2FbtsCSAX2Vt8%2FeRW4ZHsV3SxFN8E8jLNsv1%2Fimg.png" alt="clock" width="450"/>

<br/>

### 최종 저장 장치 계층 구조
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBZMsM%2FbtsCZ862vlw%2FnGejLqMrVzZk9S0J2iWua0%2Fimg.png" alt="clock" width="450"/>

<br/>

### 참조 지역성의 원리

    캐시 메모리는 메모리보다 용량이 작으므로 CPU가 자주 사용할 만한 데이터를 예측해 저장하는 것


1. 캐시 히트 : 예측이 맞을 경우 (CPU가 캐시 메모리에 저장된 값을 활용할 경우)
2. 캐시 미스 : 예측이 틀릴 경우 (CPU가 메모리에 접근해야 하는 경우)


* 캐시 적중률 :  캐시 히트 횟수 / (캐시 히트 횟수 + 캐시 미스 횟수)
   * 캐시 적중률 높이려면 CPU가 사용할 법한 데이터를 잘 예측해야 한다.
   *  CPU가 메모리 접근 시 주된 경향 바탕으로 원리 만듦
        *   최근 접근했던 메모리 공간에 다시 접근하려는 경향
        *   접근한 메모리 공간 근처를 접근하려는 경향 


<br/>

#### 🔗 참고
* [[컴퓨터 공학 기초 강의] 15강. RAM의 특성과 종류](https://youtu.be/VO0RQAA7KYc?feature=shared)
* [[컴퓨터 공학 기초 강의] 16강. 메모리의 주소 공간-물리 주소와 논리 주소](https://youtu.be/_mQNCRA1fVA?feature=shared)
* [[컴퓨터 공학 기초 강의] 17강. 캐시 메모리](https://youtu.be/qLCP0PwRp_w?feature=shared)
