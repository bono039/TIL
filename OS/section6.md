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

#### 🔗 참고
* [[컴퓨터 공학 기초 강의] 15강. RAM의 특성과 종류](https://youtu.be/VO0RQAA7KYc?feature=shared)
* [[컴퓨터 공학 기초 강의] 16강. 메모리의 주소 공간-물리 주소와 논리 주소](https://youtu.be/_mQNCRA1fVA?feature=shared)
* [[컴퓨터 공학 기초 강의] 17강. 캐시 메모리](https://youtu.be/qLCP0PwRp_w?feature=shared)
